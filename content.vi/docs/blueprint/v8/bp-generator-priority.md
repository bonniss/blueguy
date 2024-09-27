---
weight: 4
title: "Generator và Priority"
---

# Generator và Priority

{{< hint warning >}}
**Phiên bản JHipster: 8.0.0**
{{< /hint >}}

## Làm việc với generator

Cấu trúc thư mục của một blueprint điển hình như sau:

```txt
myblueprint
├── README.md
├── cli
│   └── cli.cjs
├── generators
│   ├── java
│   ├── liquibase
│   └── server
├── package-lock.json
├── package.json
├── tsconfig.json
└── vitest.config.ts
```

- `generators` là thư mục quan trọng nhất trong blueprint, chứa các bộ sinh code thành phần. Mỗi generator có thể override một generator của Jhipster hoặc mới hoàn toàn. Ở ví dụ trên, `myblueprint` nhắm tới override 3 generator của Jhipster là `java`, `liquibase` và `server`.
- `cli` chứa code để cho phép gọi `myblueprint` trên terminal.

Về mặt kỹ thuật, bản thân blueprint đã là một generator Yeoman, do đó tài liệu của JHipster dùng khái niệm `sub-generator` để nói về các bộ sinh code thành phần trong thư mục `generators`. Để ngắn gọn, ta quy ước gọi các bộ sinh code thành phần này là `SG`.

{{< hint info >}}
Xem lại ["Nguyên lý sub-generator"](/docs/blueprint/v8/bp-fundamentals/#nguyên-lý-sub-generator).
{{< /hint >}}

Xem xét SG `server`, cấu trúc thư mục như sau:

```txt
server
├── __snapshots__
│   └── generator.spec.mjs.snap
├── command.mjs
├── generator.mjs
├── generator.spec.mjs
├── index.mjs
└── templates
    ├── README.md.jhi.spring-boot.ejs
    ├── _global_partials_entity_
    ├── build.gradle.ejs
    ├── checkstyle.xml.ejs
    ├── gradle
    ├── gradle.properties.ejs
    ├── micro_services_architecture.md
    ├── npmw
    ├── npmw.cmd
    ├── package.json.ejs
    ├── pom.xml.ejs
    ├── reactive
    ├── settings.gradle.ejs
    └── src
```

Ta quan tâm nhất đến 2 thành phần:

- `generator.mjs`: chủ yếu logic nằm ở đây, chia theo priority.
- `templates`: biểu mẫu.

{{< hint info >}}
Xem lại [priority](/docs/fundamentals/yeoman/method-priority.md) trong Yeoman.
{{< /hint >}}

Nội dung giản lược của `generator.mjs` như sau:

```js
import BaseApplicationGenerator from 'generator-jhipster/generators/base-application';
import command from './command.mjs';

export default class extends BaseApplicationGenerator {
  constructor(args, opts, features) {
    super(args, opts, { ...features, sbsBlueprint: true });
  }

   // Các getter xử lý cho từng priority
   // ...
}
```

Một SG là một class kế thừa class nền `BaseApplicationGenerator`.

## Mổ xẻ Priority

Các class nền có khai báo sẵn các thông tin cần thiết để xử lý priority. Xem chi tiết [danh sách các priority trong JHipster v8 tại đây](/docs/blueprint/v8/bp-fundamentals/#priority).

Nhìn chung, một getter để xử lý priority trong SG của một blueprint sẽ có kiểu như sau:

```ts
get [BaseApplicationGenerator.<PRIORITY_NAME>](): GenericTaskGroup<this, Definition['<priority_name>TaskParam']>
```

### PRIORITY_NAME

`PRIORITY_NAME` là constant được định nghĩa sẵn trong `BaseApplicationGenerator` như `INITIALIZING`, `WRITING`, `WRITING_ENTITIES`, `END`...

### GenericTaskGroup

Một getter priority trả về một object có kiểu `GenericTaskGroup`. Định nghĩa type [tại đây](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/base/tasks.d.mts#L15):

```ts
import type { Control } from './types.mjs';

export type ControlTaskParam = {
  control: Control & Record<string, boolean | string | object>;
};

export type GenericSourceTypeDefinition<SourceType = unknown> = { sourceType: SourceType };

export type SourceTaskParam<Definition extends GenericSourceTypeDefinition> = {
  source: Definition['sourceType'];
};

export type GenericTask<ThisType, Arg1Type> = (this: ThisType, arg1: Arg1Type) => unknown;

export type GenericTaskGroup<ThisType, Arg1Type = ControlTaskParam> = Record<string, GenericTask<ThisType, Arg1Type>>;

export type BaseGeneratorDefinition<Definition extends GenericSourceTypeDefinition = GenericSourceTypeDefinition> = Record<
  | 'initializingTaskParam'
  | 'promptingTaskParam'
  | 'configuringTaskParam'
  | 'composingTaskParam'
  | 'loadingTaskParam'
  | 'defaultTaskParam'
  | 'writingTaskParam'
  | 'postWritingTaskParam'
  | 'preConflictsTaskParam'
  | 'installTaskParam'
  | 'postInstallTaskParam'
  | 'endTaskParam',
  ControlTaskParam
> &
  Record<'preparingTaskParam' | 'postPreparingTaskParam' | 'postWritingTaskParam', SourceTaskParam<Definition>>;
```

Trong class nền `BaseApplicationGenerator`, mỗi priority có một số hàm phụ trợ để đơn giản hoá việc trả về kết quả cho getter priority. Mỗi priority luôn có một "hàm tự thân" có cùng tên với priority, như hàm tự thân của priority `initializing` sau đây:

```ts
/**
 * Priority API stub for blueprints.
 *
 * Initializing priority is used to show logo and tasks related to preparing for prompts, like loading constants.
 */
get initializing(): GenericTaskGroup<this, Definition['initializingTaskParam']>;
```

Trong SG của blueprint, ta có thể gọi hàm này thông qua `super`:

```js
get [BaseApplicationGenerator.INITIALIZING]() {
  return super.initializing;
}

get [BaseApplicationGenerator.WRITING_ENTITIES]() {
  return super.writingEntities
}
```

Việc trả về hàm tự thân của priority đơn giản là giữ y nguyên hành vi của priority đó, "không làm gì cả".

Tất nhiên ở một priority cụ thể nào đó, ta sẽ viết logic để tuỳ biến code được sinh ra. Các hàm phụ trợ cho việc này có:

```ts
// cho priority INITIALIZING
/**
 * Utility method to get typed objects for autocomplete.
 */
asInitializingTaskGroup(taskGroup: GenericTaskGroup<this, Definition['initializingTaskParam']>): GenericTaskGroup<this, Definition['initializingTaskParam']>;

// cho priority PROMPTING
/**
 * Utility method to get typed objects for autocomplete.
 */
asPromptingTaskGroup(taskGroup: GenericTaskGroup<this, Definition['promptingTaskParam']>): GenericTaskGroup<this, Definition['promptingTaskParam']>;

// cho priority POST_WRITING
/**
 * Utility method to get typed objects for autocomplete.
 */
asPostWritingTaskGroup(taskGroup: GenericTaskGroup<this, Definition['postWritingTaskParam']>):
GenericTaskGroup<this, Definition['postWritingTaskParam']>;

// tương tự cho các priority còn lại
```

Thay vì trả về trực tiếp object ở mỗi getter, ta truyền object kết quả vào hàm `as<PriorityName>Taskgroup` này. Do các hàm phụ trợ này đã được Jhipster định nghĩa type rõ ràng, autocompletion trên các IDE như VSCode hoạt động được ngay mà bạn không cần khai báo thêm type nào như khi tự khai báo một object.

```ts
get [BaseApplicationGenerator.WRITING_ENTITIES]() {
  return this.asWritingEntitiesTaskGroup({
    // Một task function có cần đến tham số cung cấp bởi JHipster
    async task1(args) {
      console.info(args)
    },
    // Một task function khác không cần đến tham số nào
    async task2() {
      console.info('do nothing')
    }
  });
}

get [BaseApplicationGenerator.END]() {
  // hoặc không làm gì với bằng object rỗng.
  return this.asEndTaskGroup({});
}
```

### GenericTask

Như đã thấy ở trên, mỗi getter phải trả về một `GenericTaskGroup`, bản chất là một `Record<string, GenericTask>`, với `GenericTask` là:

```ts
type GenericTask<ThisType, Arg1Type> = (this: ThisType, arg1: Arg1Type) => unknown;
```

Mỗi `GenericTask` (mỗi entry) trong `GenericTaskGroup` (object) là một function. Kiểu trả về `unknown` thể hiện function có thể trả về kiểu tuỳ ý kể cả `Promise`, nên `async function` hoàn toàn hợp lệ.

#### Tham số đầu vào `ThisType`

Rất nhanh chóng ta sẽ cần đến các thông tin về ứng dụng, entity, database... mà ta đã khai báo thông qua JDL để sử dụng trong blueprint. Các thông tin đó được JHipster cung cấp như tham số đầu vào đầu tiên cho mỗi `GenericTask`. Tại sao lại là `ThisType`? Vì [theo định nghĩa](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/base/generator.mts#L173) thì mỗi `GenericTaskGroup`, dẫn đến mỗi `GenericTask`, đều nhận vào `this`, _là instance của class generator hiện tại_, làm tham số đầu tiên.

```ts
/**
  * Utility method to get typed objects for autocomplete.
  */
asPromptingTaskGroup(
  taskGroup: GenericTaskGroup<this, Definition['promptingTaskParam']>,
): GenericTaskGroup<this, Definition['promptingTaskParam']> {
  return taskGroup;
}

/**
 * Utility method to get typed objects for autocomplete.
 */
asConfiguringTaskGroup(
  taskGroup: GenericTaskGroup<this, Definition['configuringTaskParam']>,
): GenericTaskGroup<this, Definition['configuringTaskParam']> {
  return taskGroup;
}
```

Vì thế `ThisType` có chứa tất cả những field và method của các class base.

{{< mermaid class="text-center" >}}
flowchart LR
  subgraph JHipster base classes
    CoreGenerator --> YeomanGenerator
    BaseGenerator --> CoreGenerator
    BaseApplicationGenerator --> BaseGenerator
  end
{{< /mermaid >}}

Chi tiết mã nguồn:

- [CoreGenerator](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/base-core/generator.mts#L90)
- [BaseGenerator](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/base/generator.mts#L42)
- [BaseApplicationGenerator](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/base-application/generator.mts#L80)

#### Trường mô tả vạn năng `application`

JHipster [gom các thông tin các định nghĩa ứng dụng lấy từ JDL, JHipster, Yeoman gom lại vào trường `application`](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/app/support/config.mts#L74) nằm trong object tham số đầu tiên truyền cho function `GenericTask`.

Xét JDL sau đây:

```jdl
application {
  config {
    baseName acme
    applicationType monolith
    packageName com.lorem.acme
    authenticationType jwt
    prodDatabaseType postgresql
    devDatabaseType postgresql
    buildTool gradle

    enableTranslation false
    languages []
    skipClient true
  }

  entities *
  dto * with mapstruct
}

enum Status {
  // 0. Chưa duyệt
  PENDING_NEW ("0")
  // 1. Đã duyệt
  APPROVED_NEW ("1")
  // 2. Sửa chờ duyệt
  PENDING_EDIT ("2")
  // 3. Đã sửa  (sửa đã duyệt)
  APPROVED_EDIT ("3")
  // 4. Xóa chờ duyệt
  PENDING_DELETE ("4")
  // 9. Đã xóa
  APPROVED_DELETE ("9")
}

enum Gender {
  MALE ("1"), FEMALE ("2")
}

entity Foo {
  fooName String
  status Integer
  gender Gender
}

enum Visibility {
  ONLINE, OFFLINE
}

entity Bar {
  /**
   * Custom annotation, tend to make different name for Id field
   */
  @Id
  barId Long

  barName String
  visibility Visibility
}
relationship ManyToOne {
  Foo{bar} to Bar
}

service * with serviceClass
paginate all with pagination
filter all
```

Trong blueprint ta thử log tham số đầu tiên ra:

```js
get [BaseApplicationGenerator.POST_WRITING]() {
  return this.asPostWritingTaskGroup({
    logThisType(thisType) {
      console.info(thisType)
    },
  })
}
```

<details>
  <summary>Chi tiết log</summary>

```js
{
  application: {
    nodeDependencies: {
      prettier: '3.0.3',
      'prettier-plugin-java': '2.3.1',
      'prettier-plugin-packagejson': '2.4.6',
      concurrently: '8.2.2',
      husky: '8.0.3',
      'lint-staged': '15.0.2',
      npm: '10.2.2',
      'wait-on': '7.0.1'
    },
    baseName: 'acme',
    projectDescription: 'Description for Acme',
    applicationType: 'monolith',
    nodeVersion: '18.18.2',
    jhipsterVersion: '8.0.0',
    reactive: false,
    jhiPrefix: 'jhi',
    skipFakeData: true,
    entitySuffix: '',
    dtoSuffix: 'DTO',
    skipCheckLengthOfIdentifier: false,
    microfrontend: false,
    microfrontends: undefined,
    skipServer: undefined,
    skipCommitHook: true,
    skipClient: true,
    prettierJava: undefined,
    pages: [],
    skipJhipsterDependencies: true,
    withAdminUi: true,
    gatewayServerPort: undefined,
    capitalizedBaseName: 'Acme',
    dasherizedBaseName: 'acme',
    humanizedBaseName: 'Acme',
    authenticationType: 'jwt',
    rememberMeKey: undefined,
    jwtSecretKey: 'ZGJlMGRhODVlMDUzZGFkNzc2YWI0YTkxNDA0NTlhMDg2NDkyYjIwNWZkOWM2ZjU1MDYxNzk5NjllYjBiNzc1YWU0MTVhYTk0MmI3ZjE3YTYxMjBmMDFiMjAzZWFkN2UxYzUwN2M3MmRmN2JkMGQ5NzAzMjY4MThhY2YwZjExMzk=',
    fakerSeed: undefined,
    skipUserManagement: false,
    blueprints: [ [Object] ],
    testFrameworks: [],
    enableTranslation: false,
    nativeLanguage: 'en',
    nativeLanguageDefinition: {
      rtl: false,
      javaLocaleMessageSourceSuffix: 'en',
      angularLocale: 'en',
      dayjsLocale: 'en',
      fakerjsLocale: 'en',
      name: 'English',
      displayName: 'English',
      languageTag: 'en'
    },
    enableI18nRTL: false,
    clientFramework: 'no',
    clientTestFrameworks: undefined,
    clientRootDir: '',
    clientPackageManager: 'npm',
    clientTheme: undefined,
    clientThemeVariant: undefined,
    devServerPort: undefined,
    clientSrcDir: 'src/main/webapp/',
    clientTestDir: 'src/test/javascript/',
    srcMainJava: 'src/main/java/',
    srcMainResources: 'src/main/resources/',
    srcMainWebapp: 'src/main/webapp/',
    srcTestJava: 'src/test/java/',
    srcTestResources: 'src/test/resources/',
    srcTestJavascript: 'src/test/javascript/',
    packageName: 'com.lorem.acme',
    packageFolder: 'com/lorem/acme/',
    serverPort: 8080,
    buildTool: 'gradle',
    databaseType: 'sql',
    databaseMigration: undefined,
    devDatabaseType: 'postgresql',
    prodDatabaseType: 'postgresql',
    incrementalChangelog: undefined,
    searchEngine: 'no',
    cacheProvider: 'ehcache',
    enableHibernateCache: true,
    serviceDiscoveryType: 'no',
    enableSwaggerCodegen: false,
    messageBroker: 'no',
    websocket: 'no',
    embeddableLaunchScript: undefined,
    enableGradleEnterprise: false,
    gradleEnterpriseHost: undefined,
    gradleVersion: '8.4',
    javaVersion: '17',
    backendType: 'Java',
    packageInfoJavadocs: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ],
    javaDependencies: {
      'spring-boot': '3.1.5',
      hibernate: '6.2.13.Final',
      cassandra: '4.15.0',
      'archunit-junit5': '1.1.0',
      awaitility: '4.2.0',
      'blockhound-junit-platform': '1.0.8.RELEASE',
      'commons-beanutils': '1.9.4',
      gatling: '3.9.5',
      h2: '2.2.224',
      'hazelcast-hibernate53': '5.1.0',
      'hazelcast-spring': '5.3.5',
      'jackson-databind-nullable': '0.2.6',
      'jaxb-runtime': '4.0.4',
      'junit-platform-launcher': '1.10.0',
      liquibase: '4.24.0',
      mapstruct: '1.5.5.Final',
      'micrometer-context-propagation': '1.0.6',
      picocli: '4.7.5',
      'spring-pulsar': '0.2.0',
      typesafe: '1.4.3',
      'validation-api': '3.0.2',
      checkstyle: '10.12.4',
      'checksum-maven-plugin': 1.11,
      'frontend-maven-plugin': '1.14.2',
      'gatling-maven-plugin': '4.6.0',
      'git-commit-id-maven-plugin': '7.0.0',
      'jib-maven-plugin': '3.4.0',
      'lifecycle-mapping': '1.0.0',
      'jacoco-maven-plugin': '0.8.11',
      'maven-antrun-plugin': '3.1.0',
      'maven-checkstyle-plugin': '3.3.1',
      'maven-clean-plugin': '3.3.2',
      'maven-compiler-plugin': '3.11.0',
      'maven-eclipse-plugin': 2.1,
      'maven-enforcer-plugin': '3.4.1',
      'maven-failsafe-plugin': '3.2.1',
      'maven-idea-plugin': '2.2.1',
      'maven-jar-plugin': '3.3.0',
      'maven-javadoc-plugin': '3.6.0',
      'maven-resources-plugin': '3.3.1',
      'maven-site-plugin': '3.12.1',
      'maven-surefire-plugin': '3.2.1',
      'maven-war-plugin': '3.4.0',
      'modernizer-maven-plugin': '2.7.0',
      'nohttp-checkstyle': '0.0.11',
      'openapi-generator-maven-plugin': '7.0.1',
      'properties-maven-plugin': '1.2.1',
      'sonar-maven-plugin': '3.10.0.2594',
      'spotless-maven-plugin': '2.40.0',
      'gradle-git-properties': '2.4.1',
      'node-gradle': '7.0.1',
      'gradle-liquibase': '2.2.0',
      'gradle-sonarqube': '4.4.1.3373',
      'spotless-gradle-plugin': '6.22.0',
      'gradle-modernizer-plugin': '1.9.0',
      'gradle-enterprise': '3.15.1',
      'common-custom-user-data-gradle-plugin': '1.12',
      'gatling-gradle': '3.9.5.6'
    },
    dockerContainers: {
      elasticsearchTag: '8.7.1',
      elasticsearchImage: 'docker.elastic.co/elasticsearch/elasticsearch',
      elasticsearch: 'docker.elastic.co/elasticsearch/elasticsearch:8.7.1',
      'jhipster/jhipster-registry': 'jhipster/jhipster-registry:v7.4.0',
      jhipsterRegistry: 'jhipster/jhipster-registry:v7.4.0',
      jhipsterRegistryTag: 'v7.4.0',
      jhipsterRegistryImage: 'jhipster/jhipster-registry',
      'jhipster/jhipster-control-center': 'jhipster/jhipster-control-center:v0.5.0',
      jhipsterControlCenter: 'jhipster/jhipster-control-center:v0.5.0',
      jhipsterControlCenterTag: 'v0.5.0',
      jhipsterControlCenterImage: 'jhipster/jhipster-control-center',
      'jhipster/consul-config-loader': 'jhipster/consul-config-loader:v0.4.1',
      consulConfigLoader: 'jhipster/consul-config-loader:v0.4.1',
      consulConfigLoaderTag: 'v0.4.1',
      consulConfigLoaderImage: 'jhipster/consul-config-loader',
      postgres: 'postgres:16.0',
      postgresTag: '16.0',
      postgresImage: 'postgres',
      postgresql: 'postgres:16.0',
      postgresqlTag: '16.0',
      postgresqlImage: 'postgres',
      'quay.io/keycloak/keycloak': 'quay.io/keycloak/keycloak:22.0.5',
      keycloak: 'quay.io/keycloak/keycloak:22.0.5',
      keycloakTag: '22.0.5',
      keycloakImage: 'quay.io/keycloak/keycloak',
      mysql: 'mysql:8.2.0',
      mysqlTag: '8.2.0',
      mysqlImage: 'mysql',
      mariadb: 'mariadb:11.1.2',
      mariadbTag: '11.1.2',
      mariadbImage: 'mariadb',
      mongo: 'mongo:7.0.2',
      mongoTag: '7.0.2',
      mongoImage: 'mongo',
      mongodb: 'mongo:7.0.2',
      mongodbTag: '7.0.2',
      mongodbImage: 'mongo',
      'couchbase/server': 'couchbase/server:7.2.2',
      couchbase: 'couchbase/server:7.2.2',
      couchbaseTag: '7.2.2',
      couchbaseImage: 'couchbase/server',
      cassandra: 'cassandra:3.11.14',
      cassandraTag: '3.11.14',
      cassandraImage: 'cassandra',
      'mcr.microsoft.com/mssql/server': 'mcr.microsoft.com/mssql/server:2019-CU16-GDR1-ubuntu-20.04',
      mssql: 'mcr.microsoft.com/mssql/server:2019-CU16-GDR1-ubuntu-20.04',
      mssqlTag: '2019-CU16-GDR1-ubuntu-20.04',
      mssqlImage: 'mcr.microsoft.com/mssql/server',
      neo4j: 'neo4j:5.13.0',
      neo4J: 'neo4j:5.13.0',
      neo4JTag: '5.13.0',
      neo4JImage: 'neo4j',
      'hazelcast/management-center': 'hazelcast/management-center:5.3.3',
      hazelcast: 'hazelcast/management-center:5.3.3',
      hazelcastTag: '5.3.3',
      hazelcastImage: 'hazelcast/management-center',
      memcached: 'memcached:1.6.22-alpine',
      memcachedTag: '1.6.22-alpine',
      memcachedImage: 'memcached',
      redis: 'redis:7.2.2',
      redisTag: '7.2.2',
      redisImage: 'redis',
      'confluentinc/cp-kafka': 'confluentinc/cp-kafka:7.5.1',
      kafka: 'confluentinc/cp-kafka:7.5.1',
      kafkaTag: '7.5.1',
      kafkaImage: 'confluentinc/cp-kafka',
      'confluentinc/cp-zookeeper': 'confluentinc/cp-zookeeper:7.5.1',
      zookeeper: 'confluentinc/cp-zookeeper:7.5.1',
      zookeeperTag: '7.5.1',
      zookeeperImage: 'confluentinc/cp-zookeeper',
      'apachepulsar/pulsar': 'apachepulsar/pulsar:3.0.1',
      pulsar: 'apachepulsar/pulsar:3.0.1',
      pulsarTag: '3.0.1',
      pulsarImage: 'apachepulsar/pulsar',
      sonarqube: 'sonarqube:10.2.1-community',
      sonarqubeTag: '10.2.1-community',
      sonarqubeImage: 'sonarqube',
      sonar: 'sonarqube:10.2.1-community',
      sonarTag: '10.2.1-community',
      sonarImage: 'sonarqube',
      'docker.io/bitnami/consul': 'docker.io/bitnami/consul:1.16.2',
      consul: 'docker.io/bitnami/consul:1.16.2',
      consulTag: '1.16.2',
      consulImage: 'docker.io/bitnami/consul',
      'prom/prometheus': 'prom/prometheus:v2.47.2',
      prometheus: 'prom/prometheus:v2.47.2',
      prometheusTag: 'v2.47.2',
      prometheusImage: 'prom/prometheus',
      'prom/alertmanager': 'prom/alertmanager:v0.26.0',
      prometheusAlertmanager: 'prom/alertmanager:v0.26.0',
      prometheusAlertmanagerTag: 'v0.26.0',
      prometheusAlertmanagerImage: 'prom/alertmanager',
      'quay.io/coreos/prometheus-operator': 'quay.io/coreos/prometheus-operator:v0.42.1',
      prometheusOperator: 'quay.io/coreos/prometheus-operator:v0.42.1',
      prometheusOperatorTag: 'v0.42.1',
      prometheusOperatorImage: 'quay.io/coreos/prometheus-operator',
      'grafana/grafana': 'grafana/grafana:10.2.0',
      grafana: 'grafana/grafana:10.2.0',
      grafanaTag: '10.2.0',
      grafanaImage: 'grafana/grafana',
      'quay.io/coreos/grafana-watcher': 'quay.io/coreos/grafana-watcher:v0.0.8',
      grafanaWatcher: 'quay.io/coreos/grafana-watcher:v0.0.8',
      grafanaWatcherTag: 'v0.0.8',
      grafanaWatcherImage: 'quay.io/coreos/grafana-watcher',
      'jenkins/jenkins': 'jenkins/jenkins:lts-jdk11',
      jenkins: 'jenkins/jenkins:lts-jdk11',
      jenkinsTag: 'lts-jdk11',
      jenkinsImage: 'jenkins/jenkins',
      'eclipse-temurin': 'eclipse-temurin:17-jre-focal',
      eclipseTemurin: 'eclipse-temurin:17-jre-focal',
      eclipseTemurinTag: '17-jre-focal',
      eclipseTemurinImage: 'eclipse-temurin',
      javaJre: 'eclipse-temurin:17-jre-focal',
      javaJreTag: '17-jre-focal',
      javaJreImage: 'eclipse-temurin',
      'swaggerapi/swagger-editor': 'swaggerapi/swagger-editor:latest',
      swaggerEditor: 'swaggerapi/swagger-editor:latest',
      swaggerEditorTag: 'latest',
      swaggerEditorImage: 'swaggerapi/swagger-editor',
      'openzipkin/zipkin': 'openzipkin/zipkin:2.24',
      zipkin: 'openzipkin/zipkin:2.24',
      zipkinTag: '2.24',
      zipkinImage: 'openzipkin/zipkin'
    },
    prettierTabWidth: 2,
    defaultPackaging: 'jar',
    defaultEnvironment: 'prod',
    MAIN_DIR: 'src/main/',
    TEST_DIR: 'src/test/',
    LOGIN_REGEX: '^(?>[a-zA-Z0-9!$&*+=?^_`{|}~.-]+@[a-zA-Z0-9-]+(?:\\\\.[a-zA-Z0-9-]+)*)|(?>[_.@A-Za-z0-9-]+)$',
    CLIENT_WEBPACK_DIR: 'webpack/',
    SERVER_MAIN_SRC_DIR: 'src/main/java/',
    SERVER_MAIN_RES_DIR: 'src/main/resources/',
    SERVER_TEST_SRC_DIR: 'src/test/java/',
    SERVER_TEST_RES_DIR: 'src/test/resources/',
    JAVA_VERSION: '17',
    JAVA_COMPATIBLE_VERSIONS: [ '17', '18', '19', '20', '21' ],
    javaCompatibleVersions: [ '17', '18', '19', '20', '21' ],
    projectVersion: '0.0.1-SNAPSHOT',
    jhipsterDependenciesVersion: '8.0.0',
    ANGULAR: 'angular',
    VUE: 'vue',
    REACT: 'react',
    jhipsterPackageJson: {
      name: 'generator-jhipster',
      version: '8.0.0',
      description: 'Spring Boot + Angular/React/Vue in one handy generator',
      keywords: [Array],
      homepage: 'https://www.jhipster.tech/',
      bugs: 'https://github.com/jhipster/generator-jhipster/issues',
      repository: [Object],
      funding: [Object],
      license: 'Apache-2.0',
      author: [Object],
      type: 'module',
      exports: [Object],
      main: './dist/generators/index.js',
      types: './dist/types/generators/index.d.ts',
      bin: [Object],
      files: [Array],
      scripts: [Object],
      dependencies: [Object],
      devDependencies: [Object],
      peerDependencies: [Object],
      peerDependenciesMeta: [Object],
      engines: [Object],
      collective: [Object]
    },
    dockerServices: [ 'app', 'postgresql' ],
    camelizedBaseName: 'acme',
    hipster: 'jhipster_family_member_2',
    lowercaseBaseName: 'acme',
    upperFirstCamelCaseBaseName: 'Acme',
    applicationTypeMonolith: true,
    applicationTypeGateway: false,
    applicationTypeMicroservice: false,
    applicationTypeAny: true,
    jhiPrefixCapitalized: 'Jhi',
    jhiPrefixDashed: 'jhi',
    gatlingTests: false,
    cucumberTests: false,
    cypressTests: false,
    endpointPrefix: '',
    authenticationTypeSession: false,
    authenticationTypeJwt: true,
    authenticationTypeOauth2: false,
    generateAuthenticationApi: true,
    generateUserManagement: true,
    generateInMemoryUserCredentials: false,
    generateBuiltInUserEntity: true,
    generateBuiltInAuthorityEntity: true,
    nodePackageManager: 'npm',
    dockerServicesDir: 'src/main/docker/',
    clientFrameworkAngular: false,
    clientFrameworkReact: false,
    clientFrameworkVue: false,
    clientFrameworkNo: true,
    clientFrameworkAny: false,
    clientTestFrameworksCypress: false,
    clientTestFrameworksAny: true,
    clientThemeNone: false,
    clientThemePrimary: false,
    clientThemeLight: false,
    clientThemeDark: false,
    frontendAppName: 'acmeApp',
    prodDatabaseTypePostgresql: true,
    prodDatabaseTypeMssql: false,
    devDatabaseTypeH2Any: false,
    prodDatabaseTypeMariadb: false,
    communicationSpringWebsocket: false,
    searchEngineNo: true,
    searchEngineAny: false,
    searchEngineCouchbase: false,
    searchEngineElasticsearch: false,
    messageBrokerKafka: false,
    messageBrokerPulsar: false,
    messageBrokerAny: false,
    buildToolGradle: true,
    buildToolMaven: false,
    buildToolUnknown: false,
    cacheProviderNo: false,
    cacheProviderCaffeine: false,
    cacheProviderEhcache: true,
    cacheProviderHazelcast: false,
    cacheProviderInfinispan: false,
    cacheProviderMemcached: false,
    cacheProviderRedis: false,
    cacheProviderAny: true,
    databaseTypeNo: false,
    databaseTypeSql: true,
    databaseTypeCassandra: false,
    databaseTypeCouchbase: false,
    databaseTypeMongodb: false,
    databaseTypeNeo4j: false,
    databaseTypeAny: true,
    databaseMigrationLiquibase: true,
    javaPackageSrcDir: 'src/main/java/com/lorem/acme/',
    javaPackageTestDir: 'src/test/java/com/lorem/acme/',
    temporaryDir: 'build/',
    clientDistDir: 'build/resources/main/static/',
    authenticationUsesCsrf: false,
    imperativeOrReactive: 'imperative',
    prodDatabaseTypeMysql: false,
    prodDatabaseTypeOracle: false,
    devDatabaseTypeH2Disk: false,
    devDatabaseTypeH2Memory: false,
    devDatabaseTypeMariadb: false,
    devDatabaseTypeMssql: false,
    devDatabaseTypeMysql: false,
    devDatabaseTypeOracle: false,
    devDatabaseTypePostgresql: true,
    devDatabaseTypePostgres: true,
    prodHibernateDialect: 'org.hibernate.dialect.PostgreSQLDialect',
    prodJdbcDriver: 'org.postgresql.Driver',
    prodDatabaseUsername: 'acme',
    prodDatabasePassword: '',
    prodDatabaseName: 'acme',
    prodJdbcUrl: 'jdbc:postgresql://localhost:5432/acme',
    prodLiquibaseUrl: 'jdbc:postgresql://localhost:5432/acme',
    devJdbcUrl: 'jdbc:postgresql://localhost:5432/acme',
    devLiquibaseUrl: 'jdbc:postgresql://localhost:5432/acme',
    devR2dbcUrl: undefined,
    devHibernateDialect: 'org.hibernate.dialect.PostgreSQLDialect',
    devJdbcDriver: 'org.postgresql.Driver',
    devDatabaseUsername: 'acme',
    devDatabasePassword: '',
    devDatabaseName: 'acme',
    serviceDiscoveryAny: false,
    serviceDiscoveryConsul: false,
    serviceDiscoveryEureka: false,
    mainClass: 'AcmeApp',
    jhiTablePrefix: 'jhi',
    mainJavaDir: 'src/main/java/',
    mainJavaPackageDir: 'src/main/java/com/lorem/acme/',
    mainJavaResourceDir: 'src/main/resources/',
    testJavaDir: 'src/test/java/',
    testJavaPackageDir: 'src/test/java/com/lorem/acme/',
    testResourceDir: 'src/test/resources/',
    srcMainDir: 'src/main/',
    srcTestDir: 'src/test/',
    backendTypeSpringBoot: true,
    backendTypeJavaAny: true,
    useNpmWrapper: false,
    documentationArchiveUrl: 'https://www.jhipster.tech/documentation-archive/v8.0.0',
    prettierExtensions: 'md,json,yml,html,java',
    DOCUMENTATION_URL: 'https://www.jhipster.tech',
    DOCUMENTATION_ARCHIVE_PATH: '/documentation-archive/',
    addSpringMilestoneRepository: false,
    devDatabaseExtraOptions: '',
    prodDatabaseExtraOptions: '',
    liquibaseDefaultSchemaName: '',
    liquibaseAddH2Properties: false,
    user: {
      name: 'User',
      builtIn: true,
      entityTableName: 'jhi_user',
      relationships: [],
      fields: [Array],
      dto: true,
      adminUserDto: 'AdminUserDTO',
      builtInUser: true,
      applicationType: 'monolith',
      baseName: 'acme',
      frontendAppName: 'acmeApp',
      authenticationType: 'jwt',
      reactive: false,
      microfrontend: false,
      clientFramework: 'no',
      databaseType: 'sql',
      prodDatabaseType: 'postgresql',
      skipUiGrouping: false,
      searchEngine: 'no',
      jhiPrefix: 'jhi',
      entitySuffix: '',
      dtoSuffix: 'DTO',
      packageName: 'com.lorem.acme',
      packageFolder: 'com/lorem/acme/',
      faker: [FakerWithRandexp],
      resetFakerSeed: [Function (anonymous)],
      pagination: 'no',
      anyPropertyHasValidation: false,
      service: 'no',
      jpaMetamodelFiltering: false,
      readOnly: false,
      embedded: false,
      entityAngularJSSuffix: undefined,
      fluentMethods: true,
      clientRootFolder: '',
      primaryKey: [Object],
      entityPackage: undefined,
      anyFieldHasDocumentation: false,
      existingEnum: false,
      microserviceName: undefined,
      requiresPersistableImplementation: false,
      anyFieldIsDateDerived: false,
      anyFieldIsTimeDerived: false,
      anyFieldIsInstant: false,
      anyFieldIsUUID: false,
      anyFieldIsZonedDateTime: false,
      anyFieldIsDuration: false,
      anyFieldIsLocalDate: false,
      anyFieldIsBigDecimal: false,
      anyFieldIsBlobDerived: false,
      anyFieldHasImageContentType: false,
      anyFieldHasTextContentType: false,
      anyFieldHasFileBasedContentType: false,
      fieldsContainNoOwnerOneToOne: false,
      otherRelationships: [],
      enums: [],
      fieldNameChoices: [],
      differentRelationships: {},
      useMicroserviceJson: false,
      microserviceAppName: '',
      entityNameCapitalized: 'User',
      entityClass: 'User',
      entityInstance: 'user',
      entityNamePlural: 'Users',
      dtoClass: 'UserDTO',
      dtoInstance: 'userDTO',
      persistClass: 'User',
      persistInstance: 'user',
      restClass: 'UserDTO',
      restInstance: 'userDTO',
      entityNamePluralizedAndSpinalCased: 'users',
      entityClassPlural: 'Users',
      entityInstancePlural: 'users',
      entityI18nVariant: 'default',
      entityClassHumanized: 'User',
      entityClassPluralHumanized: 'Users',
      entityFileName: 'user',
      entityFolderName: 'user',
      entityModelFileName: 'user',
      entityParentPathAddition: '',
      entityPluralFileName: 'usersundefined',
      entityServiceFileName: 'user',
      entityAngularName: 'User',
      entityAngularNamePlural: 'Users',
      entityReactName: 'User',
      entityApiUrl: 'users',
      entityStateName: 'user',
      entityUrl: 'user',
      entityTranslationKey: 'user',
      entityTranslationKeyMenu: 'user',
      i18nKeyPrefix: 'acmeApp.user',
      i18nAlertHeaderPrefix: 'acmeApp.user',
      entityApi: '',
      entityPage: 'user',
      saveUserSnapshot: false,
      generateFakeData: [Function (anonymous)],
      paginationPagination: false,
      paginationInfiniteScroll: false,
      paginationNo: true,
      dtoMapstruct: false,
      serviceImpl: false,
      serviceNo: true,
      entityJavaPackageFolder: '',
      entityAbsolutePackage: 'com.lorem.acme',
      entityAbsoluteFolder: 'com/lorem/acme/',
      entityAbsoluteClass: 'com.lorem.acme.domain.User',
      entityJavadoc: undefined,
      entityApiDescription: undefined,
      entityInstanceDbSafe: 'jhiUser',
      tsKeyType: 'number',
      relationshipsByOtherEntity: {},
      allReferences: [Array],
      otherEntities: [],
      updatableEntity: true,
      entityContainsCollectionField: false,
      otherEntityPrimaryKeyTypes: [],
      otherEntityPrimaryKeyTypesIncludesUUID: false,
      relationshipsContainEagerLoad: false,
      containsBagRelationships: false,
      implementsEagerLoadApis: false,
      eagerRelations: [],
      regularEagerRelations: [],
      reactiveEagerRelations: [],
      reactiveRegularEagerRelations: [],
      officialDatabaseType: 'SQL',
      springDataDescription: 'Spring Data JPA',
      cypressBootstrapEntities: true,
      workaroundEntityCannotBeEmpty: false,
      workaroundInstantReactiveMariaDB: false,
      relationshipsContainOtherSideIgnore: false,
      importApiModelProperty: false,
      uniqueEnums: {},
      isUsingMapsId: false,
      mapsIdAssoc: null,
      reactiveOtherEntities: Set(0) {},
      reactiveUniqueEntityTypes: [Set],
      liquibaseFakeData: [Array],
      fakeDataCount: 2,
      anyRelationshipIsOwnerSide: false,
      dtoReferences: [Array],
      otherReferences: [],
      otherDtoReferences: []
    },
    domains: [ 'com.lorem.acme' ]
  },
  control: {
    jhipsterOldVersion: '8.0.0',
    ignoreNeedlesError: undefined,
    optionsParsed: true,
    blueprintConfigured: true,
    reproducible: false,
    existingProject: true
  },
  source: {
    addTestSpringFactory: [Function (anonymous)],
    addIntegrationTestAnnotation: [Function (anonymous)],
    addLogbackMainLog: [Function (anonymous)],
    addLogbackTestLog: [Function (anonymous)],
    applyFromGradle: [Function (anonymous)],
    addGradleDependency: [Function (anonymous)],
    addGradlePlugin: [Function (anonymous)],
    addGradleMavenRepository: [Function (anonymous)],
    addGradlePluginManagement: [Function (anonymous)],
    addGradleProperty: [Function (anonymous)],
    addDockerExtendedServiceToApplicationAndServices: [Function (anonymous)],
    addDockerExtendedServiceToServices: [Function (anonymous)],
    addDockerDependencyToApplication: [Function (anonymous)],
    addEntryToCache: [Function (anonymous)],
    addEntityToCache: [Function (anonymous)],
    addLiquibaseChangelog: [Function (anonymous)],
    addLiquibaseIncrementalChangelog: [Function (anonymous)],
    addLiquibaseConstraintsChangelog: [Function (anonymous)]
  }
}
```

</details>

{{< hint info >}}

Trong hầu hết trường hợp, `application` được nhúng vào biểu mẫu.

```ejs
// `AcmeApp.java.ejs`

// <%- mainClass %>
// <%- jhiTablePrefix %>
// <%- mainJavaDir %>
// <%- mainJavaPackageDir %>
// <%- mainJavaResourceDir %>
// <%- testJavaDir %>
// <%- testJavaPackageDir %>
// <%- testResourceDir %>
// <%- srcMainDir %>
// <%- srcTestDir %>
```

Kết quả xuất ra từ biểu mẫu trên:

```java
// `AcmeApp.java`

// AcmeApp
// jhi
// src/main/java/
// src/main/java/com/lorem/acme/
// src/main/resources/
// src/test/java/
// src/test/java/com/lorem/acme/
// src/test/resources/
// src/main/
// src/test/
```

Hay ví dụ với chính biểu mẫu [từ JHipster khi cấu hình Jackson](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/server/templates/src/main/java/_package_/config/JacksonConfiguration.java.ejs#L19-L27):

```ejs
package <%= packageName %>.config;

<%_ if (databaseTypeSql && !reactive) { _%>
import com.fasterxml.jackson.datatype.hibernate6.Hibernate6Module;
import com.fasterxml.jackson.datatype.hibernate6.Hibernate6Module.Feature;
<%_ } _%>
```

{{< /hint >}}

#### `entity` và `entities`

Mỗi priority đều có vị trí vai trò riêng biệt, và được gọi vào một thời điểm phù hợp trong queue sinh code. Do đó, ngoài biến `application`, tuỳ thuộc vào priority, object tham số đầu tiên truyền vào `GenericTask` còn có thể chứa các field tiện ích khác:

|Priority|Tham số đầu tiên|
|--|--|
|INITIALIZING|{ `control` }|
|LOADING|{ `control`, `application`, `applicationDefaults` }|
|PREPARING|{ `control`, `source`, `application`, `applicationDefaults` }|
|POST_PREPARING|{ `control`, `source`, `application` }|
|CONFIGURING_EACH_ENTITY|{ `control`, `application`, `entityName`, `entityStorage`, `entityConfig` }|
|LOADING_ENTITIES|{ `control`, `application`, `entitiesToLoad` }|
|PREPARING_EACH_ENTITY|{ `control`, `application`, `description`, `entityName`, `entity` }|
|PREPARING_EACH_ENTITY_FIELD|{ `control`, `application`, `description`, `entity`, `entityName`, `field`, `fieldName` }|
|PREPARING_EACH_ENTITY_RELATIONSHIP|{ `control`, `application`, `description`, `entity`, `entityName`, `relationship`, `relationshipName` }|
|POST_PREPARING_EACH_ENTITY|{ `control`, `application`, `description`, `entityName`, `entity` }|
|WRITING|{ `control`, `configChanges`, `application` }|
|WRITING_ENTITIES|{ `control`, `application`, `entities` }|
|POST_WRITING|{ `control`, `source`, `application` }|
|POST_WRITING_ENTITIES|{ `control`, `application`, `entities`, `source` }|
|INSTALL|{ `control`, `application` }|
|END|{ `control`, `application` }|

Với các biểu mẫu hướng entity như model, service, repository, `entity` được nhúng vào biểu mẫu, tham khảo ví dụ [JHipster sinh criteria của từng entity](https://github.com/jhipster/generator-jhipster/blob/32b2d93346e68106e43161009aa754b383a33d24/generators/server/templates/src/main/java/_package_/_entityPackage_/service/criteria/_entityClass_Criteria.java.ejs#L1):

```ejs
package <%= entityAbsolutePackage %>.<%_ if(jpaMetamodelFiltering && reactive) { _%>domain<%_ } else { _%>service<%_ } _%>.criteria;

import java.io.Serializable;
import java.util.Objects;
import org.springdoc.core.annotations.ParameterObject;
import tech.jhipster.service.Criteria;
<%_ for (const field of fields) { if (field.fieldIsEnum === true) { _%>
import <%= entityAbsolutePackage %>.domain.enumeration.<%= field.fieldType %>;
<%_ } } _%>
import tech.jhipster.service.filter.*;

<%_
var effectiveRelationships = reactive ? reactiveEagerRelations : relationships;
var filterVariables = [];
var extraFilters = {};
fields.filter(field => !field.transient).forEach((field) => {
  const fieldType = field.fieldType;
  if (field.filterableField) {
    var filterType;
    if (field.fieldIsEnum) {
      filterType = fieldType + 'Filter';
      extraFilters[fieldType] = {type: filterType, superType: 'Filter<' + fieldType + '>', fieldType: fieldType};
    } else if (field.fieldTypeDuration || field.fieldTypeTemporal || field.fieldTypeCharSequence || field.fieldTypeNumeric || field.fieldTypeBoolean) {
      filterType = fieldType + 'Filter';
    } else {
      filterType = 'Filter<' + fieldType + '>';
    }
    filterVariables.push( { filterType : filterType,
      name: field.fieldName,
      type: fieldType,
      fieldInJavaBeanMethod: field.fieldInJavaBeanMethod });
  }
});
effectiveRelationships.forEach((relationship) => {
const relationshipType = relationship.otherEntity.primaryKey.type;
const referenceFilterType = '' + relationshipType + 'Filter';
// user has a String PK when using OAuth, so change relationships accordingly
let oauthAwareReferenceFilterType = referenceFilterType;
if (relationship.otherEntityUser && authenticationTypeOauth2) {
  oauthAwareReferenceFilterType = 'StringFilter';
}
filterVariables.push({ filterType : oauthAwareReferenceFilterType,
  name: relationship.relationshipFieldName + 'Id',
  type: relationshipType,
  fieldInJavaBeanMethod: relationship.relationshipNameCapitalized + 'Id' });
});
_%>
/**
 * Criteria class for the {@link <%= entityAbsoluteClass %>} entity. This class is used
 * in {@link <%= entityAbsolutePackage %>.web.rest.<%= entityClass %>Resource} to receive all the possible filtering options from
 * the Http GET request parameters.
 * For example the following could be a valid request:
 * {@code /<%= entityApiUrl %>?id.greaterThan=5&attr1.contains=something&attr2.specified=false}
 * As Spring is unable to properly convert the types, unless specific {@link Filter} class are used, we need to use
 * fix type specific filters.
 */
@ParameterObject
@SuppressWarnings("common-java:DuplicatedBlocks")
public class <%= entityClass %>Criteria implements Serializable, Criteria {
<%_ Object.keys(extraFilters).forEach((key) => {
  extraFilter = extraFilters[key]; _%>
    /**
     * Class for filtering <%= key %>
     */
    public static class <%= extraFilter.type %> extends <%- extraFilter.superType %> {

        public <%= extraFilter.type %>() {
        }

        public <%= extraFilter.type %>(<%= extraFilter.type %> filter) {
            super(filter);
        }

        @Override
        public <%= extraFilter.type %> copy() {
            return new <%= extraFilter.type %>(this);
        }

    }
<%_ }); _%>
```

<details>
  <summary>Chi tiết log `Foo`</summary>

```js
{
  applications: [ 'acme' ],
  changelogDate: '20240921173801',
  dto: 'mapstruct',
  entityTableName: 'foo',
  fields: [
    {
      fieldName: 'id',
      id: true,
      fieldNameHumanized: 'ID',
      fieldTranslationKey: 'global.field.id',
      autoGenerate: true,
      dynamic: false,
      fieldType: 'Long',
      path: [Array],
      propertyName: 'id',
      propertyNameCapitalized: 'Id',
      fieldNameCapitalized: 'Id',
      fieldNameUnderscored: 'id',
      tsType: 'number',
      entity: [Circular *1],
      fieldIsEnum: false,
      fieldWithContentType: false,
      fieldValidate: false,
      nullable: true,
      unique: false,
      createRandexp: [Function (anonymous)],
      uniqueValue: [],
      generateFakeData: [Function (anonymous)],
      relationshipsPath: [],
      reference: [Object],
      blobContentTypeText: false,
      blobContentTypeImage: false,
      blobContentTypeAny: false,
      fieldTypeBoolean: false,
      fieldTypeBigDecimal: false,
      fieldTypeDouble: false,
      fieldTypeDuration: false,
      fieldTypeFloat: false,
      fieldTypeInstant: false,
      fieldTypeInteger: false,
      fieldTypeLocalDate: false,
      fieldTypeLong: true,
      fieldTypeString: false,
      fieldTypeUUID: false,
      fieldTypeZonedDateTime: false,
      fieldTypeImageBlob: false,
      fieldTypeAnyBlob: false,
      fieldTypeTextBlob: false,
      fieldTypeBlob: false,
      fieldTypeBytes: false,
      fieldTypeByteBuffer: false,
      fieldTypeNumeric: true,
      fieldTypeBinary: false,
      fieldTypeTimed: false,
      fieldTypeCharSequence: false,
      fieldTypeTemporal: false,
      fieldValidationRequired: false,
      fieldValidationMin: false,
      fieldValidationMinLength: false,
      fieldValidationMax: false,
      fieldValidationMaxLength: false,
      fieldValidationPattern: false,
      fieldValidationUnique: false,
      fieldValidationMinBytes: false,
      fieldValidationMaxBytes: false,
      jpaGeneratedValue: 'sequence',
      jpaGeneratedValueSequence: true,
      jpaGeneratedValueIdentity: false,
      autoGenerateByService: false,
      autoGenerateByRepository: true,
      requiresPersistableImplementation: false,
      readonly: true,
      jpaSequenceGeneratorName: 'sequenceGenerator',
      liquibaseSequenceGeneratorName: 'sequence_generator',
      liquibaseCustomSequenceGenerator: false,
      fieldNameAsDatabaseColumn: 'id',
      columnName: 'id',
      fieldInJavaBeanMethod: 'Id',
      fieldValidateRulesPatternJava: undefined,
      javaFieldType: 'Long',
      javaValueSample1: '1L',
      javaValueSample2: '2L',
      javaValueGenerator: 'longCount.incrementAndGet()',
      filterableField: true,
      fieldValidateRulesPatternAngular: undefined,
      fieldValidateRulesPatternReact: undefined,
      columnType: 'bigint',
      shouldDropDefaultValue: false,
      shouldCreateContentType: false,
      loadColumnType: 'numeric'
    },
    {
      fieldName: 'fooName',
      fieldType: 'String',
      path: [Array],
      propertyName: 'fooName',
      propertyNameCapitalized: 'FooName',
      fieldNameCapitalized: 'FooName',
      fieldNameUnderscored: 'foo_name',
      fieldNameHumanized: 'Foo Name',
      fieldTranslationKey: 'acmeApp.foo.fooName',
      tsType: 'string',
      entity: [Circular *1],
      fieldIsEnum: false,
      fieldWithContentType: false,
      fieldValidate: false,
      nullable: true,
      unique: false,
      createRandexp: [Function (anonymous)],
      uniqueValue: [],
      generateFakeData: [Function (anonymous)],
      relationshipsPath: [],
      reference: [Object],
      blobContentTypeText: false,
      blobContentTypeImage: false,
      blobContentTypeAny: false,
      fieldTypeBoolean: false,
      fieldTypeBigDecimal: false,
      fieldTypeDouble: false,
      fieldTypeDuration: false,
      fieldTypeFloat: false,
      fieldTypeInstant: false,
      fieldTypeInteger: false,
      fieldTypeLocalDate: false,
      fieldTypeLong: false,
      fieldTypeString: true,
      fieldTypeUUID: false,
      fieldTypeZonedDateTime: false,
      fieldTypeImageBlob: false,
      fieldTypeAnyBlob: false,
      fieldTypeTextBlob: false,
      fieldTypeBlob: false,
      fieldTypeBytes: false,
      fieldTypeByteBuffer: false,
      fieldTypeNumeric: false,
      fieldTypeBinary: false,
      fieldTypeTimed: false,
      fieldTypeCharSequence: true,
      fieldTypeTemporal: false,
      fieldValidationRequired: false,
      fieldValidationMin: false,
      fieldValidationMinLength: false,
      fieldValidationMax: false,
      fieldValidationMaxLength: false,
      fieldValidationPattern: false,
      fieldValidationUnique: false,
      fieldValidationMinBytes: false,
      fieldValidationMaxBytes: false,
      fieldNameAsDatabaseColumn: 'foo_name',
      columnName: 'foo_name',
      fieldInJavaBeanMethod: 'FooName',
      fieldValidateRulesPatternJava: undefined,
      javaFieldType: 'String',
      javaValueSample1: '"fooName1"',
      javaValueSample2: '"fooName2"',
      javaValueGenerator: 'UUID.randomUUID().toString()',
      filterableField: true,
      fieldValidateRulesPatternAngular: undefined,
      fieldValidateRulesPatternReact: undefined,
      columnType: 'varchar(255)',
      shouldDropDefaultValue: false,
      shouldCreateContentType: false,
      loadColumnType: 'string'
    },
    {
      fieldName: 'status',
      fieldType: 'Integer',
      path: [Array],
      propertyName: 'status',
      propertyNameCapitalized: 'Status',
      fieldNameCapitalized: 'Status',
      fieldNameUnderscored: 'status',
      fieldNameHumanized: 'Status',
      fieldTranslationKey: 'acmeApp.foo.status',
      tsType: 'number',
      entity: [Circular *1],
      fieldIsEnum: false,
      fieldWithContentType: false,
      fieldValidate: false,
      nullable: true,
      unique: false,
      createRandexp: [Function (anonymous)],
      uniqueValue: [],
      generateFakeData: [Function (anonymous)],
      relationshipsPath: [],
      reference: [Object],
      blobContentTypeText: false,
      blobContentTypeImage: false,
      blobContentTypeAny: false,
      fieldTypeBoolean: false,
      fieldTypeBigDecimal: false,
      fieldTypeDouble: false,
      fieldTypeDuration: false,
      fieldTypeFloat: false,
      fieldTypeInstant: false,
      fieldTypeInteger: true,
      fieldTypeLocalDate: false,
      fieldTypeLong: false,
      fieldTypeString: false,
      fieldTypeUUID: false,
      fieldTypeZonedDateTime: false,
      fieldTypeImageBlob: false,
      fieldTypeAnyBlob: false,
      fieldTypeTextBlob: false,
      fieldTypeBlob: false,
      fieldTypeBytes: false,
      fieldTypeByteBuffer: false,
      fieldTypeNumeric: true,
      fieldTypeBinary: false,
      fieldTypeTimed: false,
      fieldTypeCharSequence: false,
      fieldTypeTemporal: false,
      fieldValidationRequired: false,
      fieldValidationMin: false,
      fieldValidationMinLength: false,
      fieldValidationMax: false,
      fieldValidationMaxLength: false,
      fieldValidationPattern: false,
      fieldValidationUnique: false,
      fieldValidationMinBytes: false,
      fieldValidationMaxBytes: false,
      fieldNameAsDatabaseColumn: 'status',
      columnName: 'status',
      fieldInJavaBeanMethod: 'Status',
      fieldValidateRulesPatternJava: undefined,
      javaFieldType: 'Integer',
      javaValueSample1: '1',
      javaValueSample2: '2',
      javaValueGenerator: 'intCount.incrementAndGet()',
      filterableField: true,
      fieldValidateRulesPatternAngular: undefined,
      fieldValidateRulesPatternReact: undefined,
      columnType: 'integer',
      shouldDropDefaultValue: false,
      shouldCreateContentType: false,
      loadColumnType: 'numeric'
    },
    {
      fieldName: 'gender',
      fieldType: 'Gender',
      fieldValues: 'MALE (1),FEMALE (2)',
      path: [Array],
      propertyName: 'gender',
      propertyNameCapitalized: 'Gender',
      fieldNameCapitalized: 'Gender',
      fieldNameUnderscored: 'gender',
      fieldNameHumanized: 'Gender',
      fieldTranslationKey: 'acmeApp.foo.gender',
      tsType: 'Gender',
      entity: [Circular *1],
      fieldIsEnum: true,
      enumFileName: 'gender',
      enumValues: [Array],
      fieldWithContentType: false,
      fieldValidate: false,
      nullable: true,
      unique: false,
      createRandexp: [Function (anonymous)],
      uniqueValue: [],
      generateFakeData: [Function (anonymous)],
      relationshipsPath: [],
      reference: [Object],
      blobContentTypeText: false,
      blobContentTypeImage: false,
      blobContentTypeAny: false,
      fieldTypeBoolean: false,
      fieldTypeBigDecimal: false,
      fieldTypeDouble: false,
      fieldTypeDuration: false,
      fieldTypeFloat: false,
      fieldTypeInstant: false,
      fieldTypeInteger: false,
      fieldTypeLocalDate: false,
      fieldTypeLong: false,
      fieldTypeString: false,
      fieldTypeUUID: false,
      fieldTypeZonedDateTime: false,
      fieldTypeImageBlob: false,
      fieldTypeAnyBlob: false,
      fieldTypeTextBlob: false,
      fieldTypeBlob: false,
      fieldTypeBytes: false,
      fieldTypeByteBuffer: false,
      fieldTypeNumeric: false,
      fieldTypeBinary: false,
      fieldTypeTimed: false,
      fieldTypeCharSequence: false,
      fieldTypeTemporal: false,
      fieldValidationRequired: false,
      fieldValidationMin: false,
      fieldValidationMinLength: false,
      fieldValidationMax: false,
      fieldValidationMaxLength: false,
      fieldValidationPattern: false,
      fieldValidationUnique: false,
      fieldValidationMinBytes: false,
      fieldValidationMaxBytes: false,
      fieldNameAsDatabaseColumn: 'gender',
      columnName: 'gender',
      fieldInJavaBeanMethod: 'Gender',
      fieldValidateRulesPatternJava: undefined,
      javaFieldType: 'Gender',
      filterableField: true,
      fieldValidateRulesPatternAngular: undefined,
      fieldValidateRulesPatternReact: undefined,
      columnType: 'varchar(255)',
      shouldDropDefaultValue: false,
      shouldCreateContentType: false,
      loadColumnType: 'string'
    }
  ],
  jpaMetamodelFiltering: true,
  name: 'Foo',
  pagination: 'pagination',
  relationships: [
    {
      relationshipSide: 'left',
      relationshipType: 'many-to-one',
      otherEntityName: 'bar',
      relationshipName: 'bar',
      otherEntity: [Object],
      ownerSide: true,
      otherEntityField: 'id',
      relationshipLeftSide: true,
      relationshipRightSide: false,
      collection: false,
      relationshipOneToOne: false,
      relationshipOneToMany: false,
      relationshipManyToOne: true,
      relationshipManyToMany: false,
      otherEntityUser: false,
      relationshipUpdateBackReference: false,
      otherSideReferenceExists: false,
      otherEntityIsEmbedded: false,
      relatedField: [Object],
      otherEntityFieldCapitalized: 'Id',
      relationshipNamePlural: 'bars',
      relationshipFieldName: 'bar',
      relationshipNameCapitalized: 'Bar',
      relationshipNameHumanized: 'Bar',
      columnName: 'bar',
      columnNamePrefix: 'bar_',
      otherEntityNamePlural: 'bars',
      otherEntityNameCapitalized: 'Bar',
      otherEntityTableName: 'bar',
      relationshipFieldNamePlural: 'bars',
      relationshipNameCapitalizedPlural: 'Bars',
      otherEntityNameCapitalizedPlural: 'Bars',
      propertyName: 'bar',
      propertyNameCapitalized: 'Bar',
      otherEntityAngularName: 'Bar',
      otherEntityStateName: 'bar',
      jpaMetamodelFiltering: true,
      unique: false,
      otherEntityFileName: 'bar',
      otherEntityFolderName: 'bar',
      otherEntityClientRootFolder: '',
      otherEntityModulePath: 'bar',
      otherEntityModelName: 'bar',
      otherEntityPath: 'bar',
      nullable: true,
      shouldWriteJoinTable: false,
      reference: [Object],
      onDelete: undefined,
      onUpdate: undefined,
      shouldWriteRelationship: true,
      columnDataType: undefined,
      relationshipCollection: false,
      relationshipReferenceField: 'bar',
      bagRelationship: false,
      relationshipEagerLoad: false,
      ignoreOtherSideProperty: false,
      joinColumnNames: [Array]
    }
  ],
  searchEngine: 'no',
  service: 'serviceClass',
  applicationType: 'monolith',
  baseName: 'acme',
  frontendAppName: 'acmeApp',
  authenticationType: 'jwt',
  reactive: false,
  microfrontend: false,
  clientFramework: 'no',
  databaseType: 'sql',
  prodDatabaseType: 'postgresql',
  skipUiGrouping: false,
  jhiPrefix: 'jhi',
  entitySuffix: '',
  dtoSuffix: 'DTO',
  packageName: 'com.lorem.acme',
  packageFolder: 'com/lorem/acme/',
  jhiTablePrefix: 'jhi',
  searchEngineCouchbase: false,
  searchEngineElasticsearch: false,
  searchEngineAny: false,
  searchEngineNo: true,
  faker: [FakerWithRandexp],
  resetFakerSeed: [Function (anonymous)],
  anyPropertyHasValidation: false,
  readOnly: false,
  embedded: false,
  entityAngularJSSuffix: undefined,
  fluentMethods: true,
  clientRootFolder: '',
  primaryKey: {
    derived: false,
    name: 'id',
    hibernateSnakeCaseName: 'id',
    nameCapitalized: 'Id',
    type: 'Long',
    tsType: 'number',
    composite: false,
    relationships: [],
    ownFields: [ [Object] ],
    fields: [Getter],
    autoGenerate: [Getter],
    derivedFields: [Getter],
    ids: [Getter],
    hasUUID: false,
    hasLong: true,
    hasInteger: false,
    typeUUID: false,
    typeString: false,
    typeLong: true,
    typeInteger: false,
    typeNumeric: true
  },
  entityPackage: undefined,
  anyFieldHasDocumentation: false,
  existingEnum: false,
  microserviceName: undefined,
  requiresPersistableImplementation: false,
  anyFieldIsDateDerived: false,
  anyFieldIsTimeDerived: false,
  anyFieldIsInstant: false,
  anyFieldIsUUID: false,
  anyFieldIsZonedDateTime: false,
  anyFieldIsDuration: false,
  anyFieldIsLocalDate: false,
  anyFieldIsBigDecimal: false,
  anyFieldIsBlobDerived: false,
  anyFieldHasImageContentType: false,
  anyFieldHasTextContentType: false,
  anyFieldHasFileBasedContentType: false,
  fieldsContainNoOwnerOneToOne: false,
  otherRelationships: [],
  enums: [],
  fieldNameChoices: [],
  differentRelationships: { Bar: [ [Object] ] },
  changelogDateForRecent: 2024-09-21T17:38:01.000Z,
  useMicroserviceJson: false,
  microserviceAppName: '',
  entityNameCapitalized: 'Foo',
  entityClass: 'Foo',
  entityInstance: 'foo',
  entityNamePlural: 'Foos',
  dtoClass: 'FooDTO',
  dtoInstance: 'fooDTO',
  persistClass: 'Foo',
  persistInstance: 'foo',
  restClass: 'FooDTO',
  restInstance: 'fooDTO',
  entityNamePluralizedAndSpinalCased: 'foos',
  entityClassPlural: 'Foos',
  entityInstancePlural: 'foos',
  entityI18nVariant: 'default',
  entityClassHumanized: 'Foo',
  entityClassPluralHumanized: 'Foos',
  entityFileName: 'foo',
  entityFolderName: 'foo',
  entityModelFileName: 'foo',
  entityParentPathAddition: '',
  entityPluralFileName: 'foosundefined',
  entityServiceFileName: 'foo',
  entityAngularName: 'Foo',
  entityAngularNamePlural: 'Foos',
  entityReactName: 'Foo',
  entityApiUrl: 'foos',
  entityStateName: 'foo',
  entityUrl: 'foo',
  entityTranslationKey: 'foo',
  entityTranslationKeyMenu: 'foo',
  i18nKeyPrefix: 'acmeApp.foo',
  i18nAlertHeaderPrefix: 'acmeApp.foo',
  entityApi: '',
  entityPage: 'foo',
  saveUserSnapshot: false,
  generateFakeData: [Function (anonymous)],
  paginationPagination: true,
  paginationInfiniteScroll: false,
  paginationNo: false,
  dtoMapstruct: true,
  serviceImpl: false,
  serviceNo: false,
  entityJavaPackageFolder: '',
  entityAbsolutePackage: 'com.lorem.acme',
  entityAbsoluteFolder: 'com/lorem/acme/',
  entityAbsoluteClass: 'com.lorem.acme.domain.Foo',
  entityJavadoc: undefined,
  entityApiDescription: undefined,
  entityInstanceDbSafe: 'foo',
  tsKeyType: 'number',
  relationshipsByOtherEntity: { Bar: [ [Object] ] },
  allReferences: [
    {
      id: true,
      entity: [Circular *1],
      field: [Object],
      multiple: false,
      owned: true,
      doc: undefined,
      propertyJavadoc: [Getter],
      propertyApiDescription: [Getter],
      label: 'ID',
      name: 'id',
      type: 'Long',
      nameCapitalized: 'Id',
      path: [Array]
    },
    {
      id: undefined,
      entity: [Circular *1],
      field: [Object],
      multiple: false,
      owned: true,
      doc: undefined,
      propertyJavadoc: [Getter],
      propertyApiDescription: [Getter],
      label: 'Foo Name',
      name: 'fooName',
      type: 'String',
      nameCapitalized: 'FooName',
      path: [Array]
    },
    {
      id: undefined,
      entity: [Circular *1],
      field: [Object],
      multiple: false,
      owned: true,
      doc: undefined,
      propertyJavadoc: [Getter],
      propertyApiDescription: [Getter],
      label: 'Status',
      name: 'status',
      type: 'Integer',
      nameCapitalized: 'Status',
      path: [Array]
    },
    {
      id: undefined,
      entity: [Circular *1],
      field: [Object],
      multiple: false,
      owned: true,
      doc: undefined,
      propertyJavadoc: [Getter],
      propertyApiDescription: [Getter],
      label: 'Gender',
      name: 'gender',
      type: 'Gender',
      nameCapitalized: 'Gender',
      path: [Array]
    },
    {
      id: undefined,
      entity: [Circular *1],
      relationship: [Object],
      owned: true,
      collection: false,
      doc: undefined,
      propertyJavadoc: [Getter],
      propertyApiDescription: [Getter],
      name: 'bar',
      nameCapitalized: 'Bar',
      type: [Getter],
      path: [Array],
      relatedReference: [Object]
    }
  ],
  otherEntities: [
    {
      applications: [Array],
      changelogDate: '20240921173901',
      dto: 'mapstruct',
      entityTableName: 'bar',
      fields: [Array],
      jpaMetamodelFiltering: true,
      name: 'Bar',
      pagination: 'pagination',
      relationships: [],
      searchEngine: 'no',
      service: 'serviceClass',
      otherRelationships: [Array],
      applicationType: 'monolith',
      baseName: 'acme',
      frontendAppName: 'acmeApp',
      authenticationType: 'jwt',
      reactive: false,
      microfrontend: false,
      clientFramework: 'no',
      databaseType: 'sql',
      prodDatabaseType: 'postgresql',
      skipUiGrouping: false,
      jhiPrefix: 'jhi',
      entitySuffix: '',
      dtoSuffix: 'DTO',
      packageName: 'com.lorem.acme',
      packageFolder: 'com/lorem/acme/',
      jhiTablePrefix: 'jhi',
      searchEngineCouchbase: false,
      searchEngineElasticsearch: false,
      searchEngineAny: false,
      searchEngineNo: true,
      faker: [FakerWithRandexp],
      resetFakerSeed: [Function (anonymous)],
      anyPropertyHasValidation: false,
      readOnly: false,
      embedded: false,
      entityAngularJSSuffix: undefined,
      fluentMethods: true,
      clientRootFolder: '',
      primaryKey: [Object],
      entityPackage: undefined,
      anyFieldHasDocumentation: false,
      existingEnum: false,
      microserviceName: undefined,
      requiresPersistableImplementation: false,
      anyFieldIsDateDerived: false,
      anyFieldIsTimeDerived: false,
      anyFieldIsInstant: false,
      anyFieldIsUUID: false,
      anyFieldIsZonedDateTime: false,
      anyFieldIsDuration: false,
      anyFieldIsLocalDate: false,
      anyFieldIsBigDecimal: false,
      anyFieldIsBlobDerived: false,
      anyFieldHasImageContentType: false,
      anyFieldHasTextContentType: false,
      anyFieldHasFileBasedContentType: false,
      fieldsContainNoOwnerOneToOne: false,
      enums: [],
      fieldNameChoices: [],
      differentRelationships: {},
      changelogDateForRecent: 2024-09-21T17:39:01.000Z,
      useMicroserviceJson: false,
      microserviceAppName: '',
      entityNameCapitalized: 'Bar',
      entityClass: 'Bar',
      entityInstance: 'bar',
      entityNamePlural: 'Bars',
      dtoClass: 'BarDTO',
      dtoInstance: 'barDTO',
      persistClass: 'Bar',
      persistInstance: 'bar',
      restClass: 'BarDTO',
      restInstance: 'barDTO',
      entityNamePluralizedAndSpinalCased: 'bars',
      entityClassPlural: 'Bars',
      entityInstancePlural: 'bars',
      entityI18nVariant: 'default',
      entityClassHumanized: 'Bar',
      entityClassPluralHumanized: 'Bars',
      entityFileName: 'bar',
      entityFolderName: 'bar',
      entityModelFileName: 'bar',
      entityParentPathAddition: '',
      entityPluralFileName: 'barsundefined',
      entityServiceFileName: 'bar',
      entityAngularName: 'Bar',
      entityAngularNamePlural: 'Bars',
      entityReactName: 'Bar',
      entityApiUrl: 'bars',
      entityStateName: 'bar',
      entityUrl: 'bar',
      entityTranslationKey: 'bar',
      entityTranslationKeyMenu: 'bar',
      i18nKeyPrefix: 'acmeApp.bar',
      i18nAlertHeaderPrefix: 'acmeApp.bar',
      entityApi: '',
      entityPage: 'bar',
      saveUserSnapshot: false,
      generateFakeData: [Function (anonymous)],
      paginationPagination: true,
      paginationInfiniteScroll: false,
      paginationNo: false,
      dtoMapstruct: true,
      serviceImpl: false,
      serviceNo: false,
      entityJavaPackageFolder: '',
      entityAbsolutePackage: 'com.lorem.acme',
      entityAbsoluteFolder: 'com/lorem/acme/',
      entityAbsoluteClass: 'com.lorem.acme.domain.Bar',
      entityJavadoc: undefined,
      entityApiDescription: undefined,
      entityInstanceDbSafe: 'bar',
      tsKeyType: 'number',
      relationshipsByOtherEntity: {},
      allReferences: [Array],
      otherEntities: [],
      updatableEntity: true,
      entityContainsCollectionField: false,
      otherEntityPrimaryKeyTypes: [],
      otherEntityPrimaryKeyTypesIncludesUUID: false,
      relationshipsContainEagerLoad: false,
      containsBagRelationships: false,
      implementsEagerLoadApis: false,
      eagerRelations: [],
      regularEagerRelations: [],
      reactiveEagerRelations: [],
      reactiveRegularEagerRelations: [],
      officialDatabaseType: 'SQL',
      springDataDescription: 'Spring Data JPA',
      cypressBootstrapEntities: true,
      workaroundEntityCannotBeEmpty: false,
      workaroundInstantReactiveMariaDB: false,
      relationshipsContainOtherSideIgnore: false,
      importApiModelProperty: false,
      uniqueEnums: [Object],
      isUsingMapsId: false,
      mapsIdAssoc: null,
      reactiveOtherEntities: Set(0) {},
      reactiveUniqueEntityTypes: [Set]
    }
  ],
  updatableEntity: true,
  entityContainsCollectionField: false,
  otherEntityPrimaryKeyTypes: [ 'Long' ],
  otherEntityPrimaryKeyTypesIncludesUUID: false,
  relationshipsContainEagerLoad: false,
  containsBagRelationships: false,
  implementsEagerLoadApis: false,
  eagerRelations: [],
  regularEagerRelations: [],
  reactiveEagerRelations: [
    {
      relationshipSide: 'left',
      relationshipType: 'many-to-one',
      otherEntityName: 'bar',
      relationshipName: 'bar',
      otherEntity: [Object],
      ownerSide: true,
      otherEntityField: 'id',
      relationshipLeftSide: true,
      relationshipRightSide: false,
      collection: false,
      relationshipOneToOne: false,
      relationshipOneToMany: false,
      relationshipManyToOne: true,
      relationshipManyToMany: false,
      otherEntityUser: false,
      relationshipUpdateBackReference: false,
      otherSideReferenceExists: false,
      otherEntityIsEmbedded: false,
      relatedField: [Object],
      otherEntityFieldCapitalized: 'Id',
      relationshipNamePlural: 'bars',
      relationshipFieldName: 'bar',
      relationshipNameCapitalized: 'Bar',
      relationshipNameHumanized: 'Bar',
      columnName: 'bar',
      columnNamePrefix: 'bar_',
      otherEntityNamePlural: 'bars',
      otherEntityNameCapitalized: 'Bar',
      otherEntityTableName: 'bar',
      relationshipFieldNamePlural: 'bars',
      relationshipNameCapitalizedPlural: 'Bars',
      otherEntityNameCapitalizedPlural: 'Bars',
      propertyName: 'bar',
      propertyNameCapitalized: 'Bar',
      otherEntityAngularName: 'Bar',
      otherEntityStateName: 'bar',
      jpaMetamodelFiltering: true,
      unique: false,
      otherEntityFileName: 'bar',
      otherEntityFolderName: 'bar',
      otherEntityClientRootFolder: '',
      otherEntityModulePath: 'bar',
      otherEntityModelName: 'bar',
      otherEntityPath: 'bar',
      nullable: true,
      shouldWriteJoinTable: false,
      reference: [Object],
      onDelete: undefined,
      onUpdate: undefined,
      shouldWriteRelationship: true,
      columnDataType: undefined,
      relationshipCollection: false,
      relationshipReferenceField: 'bar',
      bagRelationship: false,
      relationshipEagerLoad: false,
      ignoreOtherSideProperty: false,
      joinColumnNames: [Array]
    }
  ],
  reactiveRegularEagerRelations: [
    {
      relationshipSide: 'left',
      relationshipType: 'many-to-one',
      otherEntityName: 'bar',
      relationshipName: 'bar',
      otherEntity: [Object],
      ownerSide: true,
      otherEntityField: 'id',
      relationshipLeftSide: true,
      relationshipRightSide: false,
      collection: false,
      relationshipOneToOne: false,
      relationshipOneToMany: false,
      relationshipManyToOne: true,
      relationshipManyToMany: false,
      otherEntityUser: false,
      relationshipUpdateBackReference: false,
      otherSideReferenceExists: false,
      otherEntityIsEmbedded: false,
      relatedField: [Object],
      otherEntityFieldCapitalized: 'Id',
      relationshipNamePlural: 'bars',
      relationshipFieldName: 'bar',
      relationshipNameCapitalized: 'Bar',
      relationshipNameHumanized: 'Bar',
      columnName: 'bar',
      columnNamePrefix: 'bar_',
      otherEntityNamePlural: 'bars',
      otherEntityNameCapitalized: 'Bar',
      otherEntityTableName: 'bar',
      relationshipFieldNamePlural: 'bars',
      relationshipNameCapitalizedPlural: 'Bars',
      otherEntityNameCapitalizedPlural: 'Bars',
      propertyName: 'bar',
      propertyNameCapitalized: 'Bar',
      otherEntityAngularName: 'Bar',
      otherEntityStateName: 'bar',
      jpaMetamodelFiltering: true,
      unique: false,
      otherEntityFileName: 'bar',
      otherEntityFolderName: 'bar',
      otherEntityClientRootFolder: '',
      otherEntityModulePath: 'bar',
      otherEntityModelName: 'bar',
      otherEntityPath: 'bar',
      nullable: true,
      shouldWriteJoinTable: false,
      reference: [Object],
      onDelete: undefined,
      onUpdate: undefined,
      shouldWriteRelationship: true,
      columnDataType: undefined,
      relationshipCollection: false,
      relationshipReferenceField: 'bar',
      bagRelationship: false,
      relationshipEagerLoad: false,
      ignoreOtherSideProperty: false,
      joinColumnNames: [Array]
    }
  ],
  officialDatabaseType: 'SQL',
  springDataDescription: 'Spring Data JPA',
  cypressBootstrapEntities: true,
  workaroundEntityCannotBeEmpty: false,
  workaroundInstantReactiveMariaDB: false,
  relationshipsContainOtherSideIgnore: false,
  importApiModelProperty: false,
  uniqueEnums: { Gender: 'Gender' },
  isUsingMapsId: false,
  mapsIdAssoc: null,
  reactiveOtherEntities: Set(1) {
    {
      applications: [Array],
      changelogDate: '20240921173901',
      dto: 'mapstruct',
      entityTableName: 'bar',
      fields: [Array],
      jpaMetamodelFiltering: true,
      name: 'Bar',
      pagination: 'pagination',
      relationships: [],
      searchEngine: 'no',
      service: 'serviceClass',
      otherRelationships: [Array],
      applicationType: 'monolith',
      baseName: 'acme',
      frontendAppName: 'acmeApp',
      authenticationType: 'jwt',
      reactive: false,
      microfrontend: false,
      clientFramework: 'no',
      databaseType: 'sql',
      prodDatabaseType: 'postgresql',
      skipUiGrouping: false,
      jhiPrefix: 'jhi',
      entitySuffix: '',
      dtoSuffix: 'DTO',
      packageName: 'com.lorem.acme',
      packageFolder: 'com/lorem/acme/',
      jhiTablePrefix: 'jhi',
      searchEngineCouchbase: false,
      searchEngineElasticsearch: false,
      searchEngineAny: false,
      searchEngineNo: true,
      faker: [FakerWithRandexp],
      resetFakerSeed: [Function (anonymous)],
      anyPropertyHasValidation: false,
      readOnly: false,
      embedded: false,
      entityAngularJSSuffix: undefined,
      fluentMethods: true,
      clientRootFolder: '',
      primaryKey: [Object],
      entityPackage: undefined,
      anyFieldHasDocumentation: false,
      existingEnum: false,
      microserviceName: undefined,
      requiresPersistableImplementation: false,
      anyFieldIsDateDerived: false,
      anyFieldIsTimeDerived: false,
      anyFieldIsInstant: false,
      anyFieldIsUUID: false,
      anyFieldIsZonedDateTime: false,
      anyFieldIsDuration: false,
      anyFieldIsLocalDate: false,
      anyFieldIsBigDecimal: false,
      anyFieldIsBlobDerived: false,
      anyFieldHasImageContentType: false,
      anyFieldHasTextContentType: false,
      anyFieldHasFileBasedContentType: false,
      fieldsContainNoOwnerOneToOne: false,
      enums: [],
      fieldNameChoices: [],
      differentRelationships: {},
      changelogDateForRecent: 2024-09-21T17:39:01.000Z,
      useMicroserviceJson: false,
      microserviceAppName: '',
      entityNameCapitalized: 'Bar',
      entityClass: 'Bar',
      entityInstance: 'bar',
      entityNamePlural: 'Bars',
      dtoClass: 'BarDTO',
      dtoInstance: 'barDTO',
      persistClass: 'Bar',
      persistInstance: 'bar',
      restClass: 'BarDTO',
      restInstance: 'barDTO',
      entityNamePluralizedAndSpinalCased: 'bars',
      entityClassPlural: 'Bars',
      entityInstancePlural: 'bars',
      entityI18nVariant: 'default',
      entityClassHumanized: 'Bar',
      entityClassPluralHumanized: 'Bars',
      entityFileName: 'bar',
      entityFolderName: 'bar',
      entityModelFileName: 'bar',
      entityParentPathAddition: '',
      entityPluralFileName: 'barsundefined',
      entityServiceFileName: 'bar',
      entityAngularName: 'Bar',
      entityAngularNamePlural: 'Bars',
      entityReactName: 'Bar',
      entityApiUrl: 'bars',
      entityStateName: 'bar',
      entityUrl: 'bar',
      entityTranslationKey: 'bar',
      entityTranslationKeyMenu: 'bar',
      i18nKeyPrefix: 'acmeApp.bar',
      i18nAlertHeaderPrefix: 'acmeApp.bar',
      entityApi: '',
      entityPage: 'bar',
      saveUserSnapshot: false,
      generateFakeData: [Function (anonymous)],
      paginationPagination: true,
      paginationInfiniteScroll: false,
      paginationNo: false,
      dtoMapstruct: true,
      serviceImpl: false,
      serviceNo: false,
      entityJavaPackageFolder: '',
      entityAbsolutePackage: 'com.lorem.acme',
      entityAbsoluteFolder: 'com/lorem/acme/',
      entityAbsoluteClass: 'com.lorem.acme.domain.Bar',
      entityJavadoc: undefined,
      entityApiDescription: undefined,
      entityInstanceDbSafe: 'bar',
      tsKeyType: 'number',
      relationshipsByOtherEntity: {},
      allReferences: [Array],
      otherEntities: [],
      updatableEntity: true,
      entityContainsCollectionField: false,
      otherEntityPrimaryKeyTypes: [],
      otherEntityPrimaryKeyTypesIncludesUUID: false,
      relationshipsContainEagerLoad: false,
      containsBagRelationships: false,
      implementsEagerLoadApis: false,
      eagerRelations: [],
      regularEagerRelations: [],
      reactiveEagerRelations: [],
      reactiveRegularEagerRelations: [],
      officialDatabaseType: 'SQL',
      springDataDescription: 'Spring Data JPA',
      cypressBootstrapEntities: true,
      workaroundEntityCannotBeEmpty: false,
      workaroundInstantReactiveMariaDB: false,
      relationshipsContainOtherSideIgnore: false,
      importApiModelProperty: false,
      uniqueEnums: [Object],
      isUsingMapsId: false,
      mapsIdAssoc: null,
      reactiveOtherEntities: Set(0) {},
      reactiveUniqueEntityTypes: [Set]
    }
  },
  reactiveUniqueEntityTypes: Set(2) { 'Bar', 'Foo' }
}
```

</details>

<details>
  <summary>Chi tiết log `Bar`</summary>

```js
{
  applications: [ 'acme' ],
  changelogDate: '20240921173901',
  dto: 'mapstruct',
  entityTableName: 'bar',
  fields: [
    {
      fieldName: 'barId',
      fieldType: 'Long',
      documentation: 'Custom annotation, tend to make different name for Id field',
      options: [Object],
      id: true,
      dynamic: false,
      path: [Array],
      propertyName: 'barId',
      propertyNameCapitalized: 'BarId',
      fieldNameCapitalized: 'BarId',
      fieldNameUnderscored: 'bar_id',
      fieldNameHumanized: 'Bar Id',
      fieldTranslationKey: 'acmeApp.bar.barId',
      tsType: 'number',
      entity: [Circular *1],
      fieldIsEnum: false,
      fieldWithContentType: false,
      fieldValidate: false,
      nullable: true,
      unique: false,
      createRandexp: [Function (anonymous)],
      uniqueValue: [],
      generateFakeData: [Function (anonymous)],
      relationshipsPath: [],
      reference: [Object],
      blobContentTypeText: false,
      blobContentTypeImage: false,
      blobContentTypeAny: false,
      fieldTypeBoolean: false,
      fieldTypeBigDecimal: false,
      fieldTypeDouble: false,
      fieldTypeDuration: false,
      fieldTypeFloat: false,
      fieldTypeInstant: false,
      fieldTypeInteger: false,
      fieldTypeLocalDate: false,
      fieldTypeLong: true,
      fieldTypeString: false,
      fieldTypeUUID: false,
      fieldTypeZonedDateTime: false,
      fieldTypeImageBlob: false,
      fieldTypeAnyBlob: false,
      fieldTypeTextBlob: false,
      fieldTypeBlob: false,
      fieldTypeBytes: false,
      fieldTypeByteBuffer: false,
      fieldTypeNumeric: true,
      fieldTypeBinary: false,
      fieldTypeTimed: false,
      fieldTypeCharSequence: false,
      fieldTypeTemporal: false,
      fieldValidationRequired: false,
      fieldValidationMin: false,
      fieldValidationMinLength: false,
      fieldValidationMax: false,
      fieldValidationMaxLength: false,
      fieldValidationPattern: false,
      fieldValidationUnique: false,
      fieldValidationMinBytes: false,
      fieldValidationMaxBytes: false,
      fieldJavadoc: '    /**\n' +
        '     * Custom annotation, tend to make different name for Id field\n' +
        '     */',
      fieldApiDescription: 'Custom annotation, tend to make different name for Id field',
      autoGenerate: true,
      jpaGeneratedValue: 'sequence',
      jpaGeneratedValueSequence: true,
      jpaGeneratedValueIdentity: false,
      autoGenerateByService: false,
      autoGenerateByRepository: true,
      requiresPersistableImplementation: false,
      readonly: true,
      jpaSequenceGeneratorName: 'sequenceGenerator',
      liquibaseSequenceGeneratorName: 'sequence_generator',
      liquibaseCustomSequenceGenerator: false,
      fieldNameAsDatabaseColumn: 'bar_id',
      columnName: 'bar_id',
      fieldInJavaBeanMethod: 'BarId',
      fieldValidateRulesPatternJava: undefined,
      javaFieldType: 'Long',
      javaValueSample1: '1L',
      javaValueSample2: '2L',
      javaValueGenerator: 'longCount.incrementAndGet()',
      filterableField: true,
      fieldValidateRulesPatternAngular: undefined,
      fieldValidateRulesPatternReact: undefined,
      columnType: 'bigint',
      shouldDropDefaultValue: false,
      shouldCreateContentType: false,
      loadColumnType: 'numeric',
      relatedByOtherEntity: true
    },
    {
      fieldName: 'barName',
      fieldType: 'String',
      path: [Array],
      propertyName: 'barName',
      propertyNameCapitalized: 'BarName',
      fieldNameCapitalized: 'BarName',
      fieldNameUnderscored: 'bar_name',
      fieldNameHumanized: 'Bar Name',
      fieldTranslationKey: 'acmeApp.bar.barName',
      tsType: 'string',
      entity: [Circular *1],
      fieldIsEnum: false,
      fieldWithContentType: false,
      fieldValidate: false,
      nullable: true,
      unique: false,
      createRandexp: [Function (anonymous)],
      uniqueValue: [],
      generateFakeData: [Function (anonymous)],
      relationshipsPath: [],
      reference: [Object],
      blobContentTypeText: false,
      blobContentTypeImage: false,
      blobContentTypeAny: false,
      fieldTypeBoolean: false,
      fieldTypeBigDecimal: false,
      fieldTypeDouble: false,
      fieldTypeDuration: false,
      fieldTypeFloat: false,
      fieldTypeInstant: false,
      fieldTypeInteger: false,
      fieldTypeLocalDate: false,
      fieldTypeLong: false,
      fieldTypeString: true,
      fieldTypeUUID: false,
      fieldTypeZonedDateTime: false,
      fieldTypeImageBlob: false,
      fieldTypeAnyBlob: false,
      fieldTypeTextBlob: false,
      fieldTypeBlob: false,
      fieldTypeBytes: false,
      fieldTypeByteBuffer: false,
      fieldTypeNumeric: false,
      fieldTypeBinary: false,
      fieldTypeTimed: false,
      fieldTypeCharSequence: true,
      fieldTypeTemporal: false,
      fieldValidationRequired: false,
      fieldValidationMin: false,
      fieldValidationMinLength: false,
      fieldValidationMax: false,
      fieldValidationMaxLength: false,
      fieldValidationPattern: false,
      fieldValidationUnique: false,
      fieldValidationMinBytes: false,
      fieldValidationMaxBytes: false,
      fieldNameAsDatabaseColumn: 'bar_name',
      columnName: 'bar_name',
      fieldInJavaBeanMethod: 'BarName',
      fieldValidateRulesPatternJava: undefined,
      javaFieldType: 'String',
      javaValueSample1: '"barName1"',
      javaValueSample2: '"barName2"',
      javaValueGenerator: 'UUID.randomUUID().toString()',
      filterableField: true,
      fieldValidateRulesPatternAngular: undefined,
      fieldValidateRulesPatternReact: undefined,
      columnType: 'varchar(255)',
      shouldDropDefaultValue: false,
      shouldCreateContentType: false,
      loadColumnType: 'string'
    },
    {
      fieldName: 'visibility',
      fieldType: 'Visibility',
      fieldValues: 'ONLINE,OFFLINE',
      path: [Array],
      propertyName: 'visibility',
      propertyNameCapitalized: 'Visibility',
      fieldNameCapitalized: 'Visibility',
      fieldNameUnderscored: 'visibility',
      fieldNameHumanized: 'Visibility',
      fieldTranslationKey: 'acmeApp.bar.visibility',
      tsType: 'Visibility',
      entity: [Circular *1],
      fieldIsEnum: true,
      enumFileName: 'visibility',
      enumValues: [Array],
      fieldWithContentType: false,
      fieldValidate: false,
      nullable: true,
      unique: false,
      createRandexp: [Function (anonymous)],
      uniqueValue: [],
      generateFakeData: [Function (anonymous)],
      relationshipsPath: [],
      reference: [Object],
      blobContentTypeText: false,
      blobContentTypeImage: false,
      blobContentTypeAny: false,
      fieldTypeBoolean: false,
      fieldTypeBigDecimal: false,
      fieldTypeDouble: false,
      fieldTypeDuration: false,
      fieldTypeFloat: false,
      fieldTypeInstant: false,
      fieldTypeInteger: false,
      fieldTypeLocalDate: false,
      fieldTypeLong: false,
      fieldTypeString: false,
      fieldTypeUUID: false,
      fieldTypeZonedDateTime: false,
      fieldTypeImageBlob: false,
      fieldTypeAnyBlob: false,
      fieldTypeTextBlob: false,
      fieldTypeBlob: false,
      fieldTypeBytes: false,
      fieldTypeByteBuffer: false,
      fieldTypeNumeric: false,
      fieldTypeBinary: false,
      fieldTypeTimed: false,
      fieldTypeCharSequence: false,
      fieldTypeTemporal: false,
      fieldValidationRequired: false,
      fieldValidationMin: false,
      fieldValidationMinLength: false,
      fieldValidationMax: false,
      fieldValidationMaxLength: false,
      fieldValidationPattern: false,
      fieldValidationUnique: false,
      fieldValidationMinBytes: false,
      fieldValidationMaxBytes: false,
      fieldNameAsDatabaseColumn: 'visibility',
      columnName: 'visibility',
      fieldInJavaBeanMethod: 'Visibility',
      fieldValidateRulesPatternJava: undefined,
      javaFieldType: 'Visibility',
      filterableField: true,
      fieldValidateRulesPatternAngular: undefined,
      fieldValidateRulesPatternReact: undefined,
      columnType: 'varchar(255)',
      shouldDropDefaultValue: false,
      shouldCreateContentType: false,
      loadColumnType: 'string'
    }
  ],
  jpaMetamodelFiltering: true,
  name: 'Bar',
  pagination: 'pagination',
  relationships: [],
  searchEngine: 'no',
  service: 'serviceClass',
  otherRelationships: [
    {
      relationshipSide: 'left',
      relationshipType: 'many-to-one',
      otherEntityName: 'bar',
      relationshipName: 'bar',
      otherEntity: [Circular *1],
      ownerSide: true,
      otherEntityField: 'barId',
      relationshipLeftSide: true,
      relationshipRightSide: false,
      collection: false,
      relationshipOneToOne: false,
      relationshipOneToMany: false,
      relationshipManyToOne: true,
      relationshipManyToMany: false,
      otherEntityUser: false,
      relationshipUpdateBackReference: false,
      otherSideReferenceExists: false,
      otherEntityIsEmbedded: false,
      relatedField: [Object],
      otherEntityFieldCapitalized: 'BarId',
      relationshipNamePlural: 'bars',
      relationshipFieldName: 'bar',
      relationshipNameCapitalized: 'Bar',
      relationshipNameHumanized: 'Bar',
      columnName: 'bar',
      columnNamePrefix: 'bar_',
      otherEntityNamePlural: 'bars',
      otherEntityNameCapitalized: 'Bar',
      otherEntityTableName: 'bar',
      relationshipFieldNamePlural: 'bars',
      relationshipNameCapitalizedPlural: 'Bars',
      otherEntityNameCapitalizedPlural: 'Bars',
      propertyName: 'bar',
      propertyNameCapitalized: 'Bar',
      otherEntityAngularName: 'Bar',
      otherEntityStateName: 'bar',
      jpaMetamodelFiltering: true,
      unique: false,
      otherEntityFileName: 'bar',
      otherEntityFolderName: 'bar',
      otherEntityClientRootFolder: '',
      otherEntityModulePath: 'bar',
      otherEntityModelName: 'bar',
      otherEntityPath: 'bar',
      nullable: true,
      shouldWriteJoinTable: false,
      reference: [Object],
      onDelete: undefined,
      onUpdate: undefined,
      shouldWriteRelationship: true,
      columnDataType: undefined,
      relationshipCollection: false,
      relationshipReferenceField: 'bar',
      bagRelationship: false,
      relationshipEagerLoad: false,
      ignoreOtherSideProperty: false,
      joinColumnNames: [Array]
    }
  ],
  applicationType: 'monolith',
  baseName: 'acme',
  frontendAppName: 'acmeApp',
  authenticationType: 'jwt',
  reactive: false,
  microfrontend: false,
  clientFramework: 'no',
  databaseType: 'sql',
  prodDatabaseType: 'postgresql',
  skipUiGrouping: false,
  jhiPrefix: 'jhi',
  entitySuffix: '',
  dtoSuffix: 'DTO',
  packageName: 'com.lorem.acme',
  packageFolder: 'com/lorem/acme/',
  jhiTablePrefix: 'jhi',
  searchEngineCouchbase: false,
  searchEngineElasticsearch: false,
  searchEngineAny: false,
  searchEngineNo: true,
  faker: [FakerWithRandexp],
  resetFakerSeed: [Function (anonymous)],
  anyPropertyHasValidation: false,
  readOnly: false,
  embedded: false,
  entityAngularJSSuffix: undefined,
  fluentMethods: true,
  clientRootFolder: '',
  primaryKey: {
    derived: false,
    name: 'barId',
    hibernateSnakeCaseName: 'bar_id',
    nameCapitalized: 'BarId',
    type: 'Long',
    tsType: 'number',
    composite: false,
    relationships: [],
    ownFields: [ [Object] ],
    fields: [Getter],
    autoGenerate: [Getter],
    derivedFields: [Getter],
    ids: [Getter],
    hasUUID: false,
    hasLong: true,
    hasInteger: false,
    typeUUID: false,
    typeString: false,
    typeLong: true,
    typeInteger: false,
    typeNumeric: true
  },
  entityPackage: undefined,
  anyFieldHasDocumentation: true,
  existingEnum: false,
  microserviceName: undefined,
  requiresPersistableImplementation: false,
  anyFieldIsDateDerived: false,
  anyFieldIsTimeDerived: false,
  anyFieldIsInstant: false,
  anyFieldIsUUID: false,
  anyFieldIsZonedDateTime: false,
  anyFieldIsDuration: false,
  anyFieldIsLocalDate: false,
  anyFieldIsBigDecimal: false,
  anyFieldIsBlobDerived: false,
  anyFieldHasImageContentType: false,
  anyFieldHasTextContentType: false,
  anyFieldHasFileBasedContentType: false,
  fieldsContainNoOwnerOneToOne: false,
  enums: [],
  fieldNameChoices: [],
  differentRelationships: {},
  changelogDateForRecent: 2024-09-21T17:39:01.000Z,
  useMicroserviceJson: false,
  microserviceAppName: '',
  entityNameCapitalized: 'Bar',
  entityClass: 'Bar',
  entityInstance: 'bar',
  entityNamePlural: 'Bars',
  dtoClass: 'BarDTO',
  dtoInstance: 'barDTO',
  persistClass: 'Bar',
  persistInstance: 'bar',
  restClass: 'BarDTO',
  restInstance: 'barDTO',
  entityNamePluralizedAndSpinalCased: 'bars',
  entityClassPlural: 'Bars',
  entityInstancePlural: 'bars',
  entityI18nVariant: 'default',
  entityClassHumanized: 'Bar',
  entityClassPluralHumanized: 'Bars',
  entityFileName: 'bar',
  entityFolderName: 'bar',
  entityModelFileName: 'bar',
  entityParentPathAddition: '',
  entityPluralFileName: 'barsundefined',
  entityServiceFileName: 'bar',
  entityAngularName: 'Bar',
  entityAngularNamePlural: 'Bars',
  entityReactName: 'Bar',
  entityApiUrl: 'bars',
  entityStateName: 'bar',
  entityUrl: 'bar',
  entityTranslationKey: 'bar',
  entityTranslationKeyMenu: 'bar',
  i18nKeyPrefix: 'acmeApp.bar',
  i18nAlertHeaderPrefix: 'acmeApp.bar',
  entityApi: '',
  entityPage: 'bar',
  saveUserSnapshot: false,
  generateFakeData: [Function (anonymous)],
  paginationPagination: true,
  paginationInfiniteScroll: false,
  paginationNo: false,
  dtoMapstruct: true,
  serviceImpl: false,
  serviceNo: false,
  entityJavaPackageFolder: '',
  entityAbsolutePackage: 'com.lorem.acme',
  entityAbsoluteFolder: 'com/lorem/acme/',
  entityAbsoluteClass: 'com.lorem.acme.domain.Bar',
  entityJavadoc: undefined,
  entityApiDescription: undefined,
  entityInstanceDbSafe: 'bar',
  tsKeyType: 'number',
  relationshipsByOtherEntity: {},
  allReferences: [
    {
      id: true,
      entity: [Circular *1],
      field: [Object],
      multiple: false,
      owned: true,
      doc: 'Custom annotation, tend to make different name for Id field',
      propertyJavadoc: [Getter],
      propertyApiDescription: [Getter],
      label: 'Bar Id',
      name: 'barId',
      type: 'Long',
      nameCapitalized: 'BarId',
      path: [Array]
    },
    {
      id: undefined,
      entity: [Circular *1],
      field: [Object],
      multiple: false,
      owned: true,
      doc: undefined,
      propertyJavadoc: [Getter],
      propertyApiDescription: [Getter],
      label: 'Bar Name',
      name: 'barName',
      type: 'String',
      nameCapitalized: 'BarName',
      path: [Array]
    },
    {
      id: undefined,
      entity: [Circular *1],
      field: [Object],
      multiple: false,
      owned: true,
      doc: undefined,
      propertyJavadoc: [Getter],
      propertyApiDescription: [Getter],
      label: 'Visibility',
      name: 'visibility',
      type: 'Visibility',
      nameCapitalized: 'Visibility',
      path: [Array]
    }
  ],
  otherEntities: [],
  updatableEntity: true,
  entityContainsCollectionField: false,
  otherEntityPrimaryKeyTypes: [],
  otherEntityPrimaryKeyTypesIncludesUUID: false,
  relationshipsContainEagerLoad: false,
  containsBagRelationships: false,
  implementsEagerLoadApis: false,
  eagerRelations: [],
  regularEagerRelations: [],
  reactiveEagerRelations: [],
  reactiveRegularEagerRelations: [],
  officialDatabaseType: 'SQL',
  springDataDescription: 'Spring Data JPA',
  cypressBootstrapEntities: true,
  workaroundEntityCannotBeEmpty: false,
  workaroundInstantReactiveMariaDB: false,
  relationshipsContainOtherSideIgnore: false,
  importApiModelProperty: true,
  uniqueEnums: { Visibility: 'Visibility' },
  isUsingMapsId: false,
  mapsIdAssoc: null,
  reactiveOtherEntities: Set(0) {},
  reactiveUniqueEntityTypes: Set(1) { 'Bar' }
}
```

</details>
