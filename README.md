# Plataforma de Configuración y Dependencias Java

Configuración Maven centralizada para los proyectos Java de `local.jarios`: gestión de dependencias, herramientas de construcción, controles de calidad y publicación de artefactos.

**Artefacto Maven:** `local.jarios:jarios-parent:1.0.15`
**Repositorio:** [contratacionmalaga/jarios-parent](https://github.com/contratacionmalaga/jarios-parent)

## Información de la versión

| Característica | Configuración |
|---|---|
| Versión del parent | **1.0.15** |
| Tipo de artefacto | `pom` |
| Java de referencia y compilación | **21** |
| Maven mínimo configurado | **3.9.16** |
| Maven distribuido mediante Wrapper | **3.9.16** |
| Maven Wrapper | **3.3.4** |
| Codificación | UTF-8 |
| Dependencias gestionadas | 40 |
| Plugins gestionados | 21 |
| Publicación | GitHub Packages |
| Fecha de revisión del inventario | 26 de septiembre de 2026 |

La versión 1.0.15 actualiza H2, Compiler, Surefire, Failsafe, Spotless, SpotBugs, Build Helper, Versions, Checkstyle, Google Java Format y la acción `setup-java`. También actualiza la propiedad de Find Security Bugs, alinea la propiedad heredable `uuid.version` con Java UUID Generator e incorpora Dependabot para proponer cambios mediante pull requests.

Las tablas siguientes muestran las versiones configuradas en esta versión. El `pom.xml` es la fuente de referencia. Cada PR de actualización debe mantener sincronizado este inventario.

## Alcance y funcionamiento

El proyecto contiene un POM padre sin código de aplicación ni módulos declarados.

- `dependencyManagement` establece versiones; cada consumidor declara las bibliotecas que necesita y sus ámbitos (`test`, `runtime`, `provided`, etc.). No se añaden todas las bibliotecas automáticamente.
- `pluginManagement` define versiones y configuraciones. La ejecución depende del ciclo de vida Maven, de la configuración del consumidor o de los perfiles activados.
- Maven Enforcer comprueba Java y Maven en `validate`.
- El perfil `quality` activa Spotless, Checkstyle y SpotBugs en `verify`.
- OWASP Dependency-Check está disponible para invocación explícita; no está vinculado a `verify`.
- El parent configura GitHub Packages como destino de publicación.

La validación de este parent no sustituye las pruebas de compatibilidad de sus consumidores. Estos pueden sobrescribir propiedades, declarar otras dependencias o resolver árboles transitivos diferentes.

## Requisitos y uso del Wrapper

Se requiere un JDK compatible con Java 21, `JAVA_HOME` correctamente configurado y acceso a Maven Central. Los paquetes propios requieren acceso autenticado a GitHub Packages. Las pruebas de consumidores que utilicen Testcontainers necesitan Docker o un entorno compatible.

Windows:

```powershell
java -version
.\mvnw.cmd -version
```

Linux y macOS:

```bash
chmod +x mvnw
./mvnw -version
```

El primer uso puede descargar Maven y sus componentes. El Wrapper no se hereda: cada consumidor debe disponer de su propio Wrapper o de una instalación Maven compatible.

## Incorporación a un proyecto consumidor

Declarar el parent en el POM:

```xml
<parent>
    <groupId>local.jarios</groupId>
    <artifactId>jarios-parent</artifactId>
    <version>1.0.15</version>
    <relativePath/>
</parent>
```

`<relativePath/>` evita buscar el parent en una ruta local implícita. La versión debe estar publicada en un repositorio configurado o instalada localmente.

Declarar las dependencias necesarias sin repetir su versión:

```xml
<dependencies>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

Para instalar este parent en el repositorio Maven local:

```powershell
.\mvnw.cmd -B -ntp clean install
```

## Dependencias gestionadas

| Coordenadas Maven | Versión | Propiedad | Finalidad |
|---|---|---|---|
| `ch.qos.logback:logback-classic` | 1.6.4 | `logback.version` | Implementación de logging |
| `com.fasterxml.jackson.core:jackson-annotations` | 2.22 | `jackson.annotations.version` | Anotaciones JSON |
| `com.fasterxml.jackson.core:jackson-core` | 2.22.3 | `jackson.version` | Procesamiento JSON |
| `com.fasterxml.jackson.core:jackson-databind` | 2.22.3 | `jackson.version` | Conversión entre Java y JSON |
| `com.fasterxml.jackson.datatype:jackson-datatype-jsr310` | 2.22.3 | `jackson.version` | Soporte JSON de fechas y horas |
| `com.sun.mail:jakarta.mail` | 2.0.2 | `jakarta.mail.version` | Servicios de correo |
| `com.zaxxer:HikariCP` | 7.1.0 | `hikaricp.version` | Pool de conexiones JDBC |
| `jakarta.persistence:jakarta.persistence-api` | 3.2.0 | `jakarta.persistence.version` | API de persistencia |
| `jakarta.xml.bind:jakarta.xml.bind-api` | 4.0.5 | `jakarta.xml.bind.version` | API XML |
| `org.glassfish.jaxb:jaxb-runtime` | 4.0.9 | `jaxb.runtime.version` | Implementación JAXB |
| `local.jarios:email-helper` | 6.1.4 | `email-helper.version` | Helper propio de correo |
| `local.jarios:encrypt-helper` | 7.0.5 | `encrypt-helper.version` | Helper propio de cifrado |
| `local.jarios:properties-helper` | 6.0.5 | `property-helper.version` | Helper propio de propiedades |
| `local.jarios:version-helper` | 6.1.4 | `version-helper.version` | Helper propio de versiones |
| `org.assertj:assertj-core` | 3.27.7 | `assertj.version` | Aserciones para pruebas |
| `org.hibernate.orm:hibernate-core` | 7.4.10.Final | `hibernate.version` | Persistencia ORM |
| `org.hibernate.orm:hibernate-hikaricp` | 7.4.10.Final | `hibernate.version` | Integración Hibernate/HikariCP |
| `org.jsoup:jsoup` | 1.23.2 | `jsoup.version` | Tratamiento HTML |
| `org.apache.logging.log4j:log4j-core` | 2.26.1 | `log4j.version` | Implementación de logging |
| `org.apache.logging.log4j:log4j-to-slf4j` | 2.26.1 | `log4j.version` | Puente Log4j hacia SLF4J |
| `com.google.code.gson:gson` | 2.14.0 | `gson.version` | Serialización JSON |
| `com.fasterxml.uuid:java-uuid-generator` | 5.2.0 | `java-uuid-generator.version` | Generación de UUID |
| `org.postgresql:postgresql` | 42.7.13 | `postgresql.version` | Controlador JDBC PostgreSQL |
| `com.h2database:h2` | 2.5.252 | `h2.version` | Base de datos H2 |
| `org.apache.commons:commons-text` | 1.15.0 | `commons.text.version` | Utilidades de texto |
| `org.apache.commons:commons-collections4` | 4.6.0 | `commons.collections.version` | Utilidades de colecciones |
| `commons-codec:commons-codec` | 1.22.1 | `commons.codec.version` | Codificación y decodificación |
| `org.testcontainers:testcontainers` | 2.0.5 | `testcontainers.version` | Pruebas con contenedores |
| `org.testcontainers:mariadb` | 1.21.4 | `testcontainers.mariadb.version` | Módulo MariaDB de Testcontainers 1.x |
| `org.testcontainers:testcontainers-mariadb` | 2.0.5 | `testcontainers.version` | Módulo MariaDB de Testcontainers 2.x |
| `org.jasypt:jasypt` | 1.9.3 | `jasypt.version` | Utilidades de cifrado |
| `jakarta.validation:jakarta.validation-api` | 3.1.1 | `jakarta.validation.version` | API de validación |
| `jakarta.annotation:jakarta.annotation-api` | 3.0.0 | `jakarta.annotation.version` | Anotaciones comunes |
| `org.junit.jupiter:junit-jupiter` | 6.1.3 | `junit.version` | Pruebas JUnit |
| `org.mariadb.jdbc:mariadb-java-client` | 3.5.10 | `mariadb.version` | Controlador JDBC MariaDB |
| `org.apache.poi:poi` | 5.5.1 | `poi.version` | Documentos Office |
| `org.apache.poi:poi-ooxml` | 5.5.1 | `poi.version` | Documentos Office OOXML |
| `org.projectlombok:lombok` | 1.18.48 | `lombok.version` | Generación de código mediante anotaciones |
| `org.reflections:reflections` | 0.10.2 | `reflections.version` | Exploración de metadatos de clases |
| `org.slf4j:slf4j-api` | 2.0.20 | `slf4j.version` | API de logging |

### Criterios de integración

- **Jackson:** `jackson.annotations.version` es independiente de `jackson.version`; no se presupone que Annotations comparta exactamente la numeración de Core, Databind y JSR310.
- **Logging:** seleccionar una configuración coherente de API, implementación y puentes. Gestionar Logback y Log4j no implica que deban activarse conjuntamente.
- **APIs Jakarta:** su gestión no incorpora necesariamente una implementación.
- **Lombok:** el consumidor debe configurar el procesamiento de anotaciones que requiera su compilación.
- **Testcontainers:** `mariadb:1.21.4` y `testcontainers-mariadb:2.0.5` son coordenadas de generaciones distintas. El núcleo gestionado es `testcontainers:2.0.5`. Se conserva la entrada antigua para los consumidores existentes, pero se debe verificar expresamente su árbol de dependencias antes de combinarla con el núcleo 2.x. La migración requiere revisar el artefacto y el código consumidor.
- **H2:** verificar las consultas y las pruebas de persistencia del consumidor al adoptar 2.5.252, especialmente si conserva bases de datos entre ejecuciones.

### Paquetes propios

| Artefacto | Versión | Repositorio |
|---|---|---|
| `local.jarios:email-helper` | 6.1.4 | [email_helper](https://github.com/contratacionmalaga/email_helper) |
| `local.jarios:encrypt-helper` | 7.0.5 | [encrypt_helper](https://github.com/contratacionmalaga/encrypt_helper) |
| `local.jarios:properties-helper` | 6.0.5 | [properties_helper](https://github.com/contratacionmalaga/properties_helper) |
| `local.jarios:version-helper` | 6.1.4 | [version_helper](https://github.com/contratacionmalaga/version_helper) |

Consultar sus repositorios para conocer las APIs y requisitos específicos. `properties-helper` utiliza la propiedad `property-helper.version`, en singular.

## Plugins de construcción

| Coordenadas Maven | Versión | Finalidad |
|---|---|---|
| `org.apache.maven.plugins:maven-antrun-plugin` | 3.2.0 | Tareas Ant |
| `org.apache.maven.plugins:maven-assembly-plugin` | 3.8.0 | Distribuciones |
| `org.apache.maven.plugins:maven-checkstyle-plugin` | 3.6.0 | Integración Checkstyle |
| `org.apache.maven.plugins:maven-clean-plugin` | 3.5.0 | Limpieza |
| `org.apache.maven.plugins:maven-compiler-plugin` | 3.16.0 | Compilación Java |
| `org.apache.maven.plugins:maven-dependency-plugin` | 3.11.0 | Inspección de dependencias |
| `org.apache.maven.plugins:maven-enforcer-plugin` | 3.6.3 | Requisitos Java/Maven |
| `org.apache.maven.plugins:maven-failsafe-plugin` | 3.6.0 | Pruebas de integración |
| `org.apache.maven.plugins:maven-jar-plugin` | 3.5.1 | Empaquetado JAR |
| `org.apache.maven.plugins:maven-javadoc-plugin` | 3.12.0 | Documentación Java |
| `org.apache.maven.plugins:maven-release-plugin` | 3.3.1 | Operaciones de release |
| `org.apache.maven.plugins:maven-shade-plugin` | 3.6.2 | Empaquetado con dependencias |
| `org.apache.maven.plugins:maven-site-plugin` | 4.0.0-M16 | Sitio Maven; versión milestone |
| `org.apache.maven.plugins:maven-source-plugin` | 3.4.0 | Empaquetado de fuentes |
| `org.apache.maven.plugins:maven-surefire-plugin` | 3.6.0 | Pruebas unitarias |
| `com.diffplug.spotless:spotless-maven-plugin` | 3.10.3 | Formato |
| `com.github.spotbugs:spotbugs-maven-plugin` | 4.10.4.1 | Análisis estático |
| `org.codehaus.mojo:build-helper-maven-plugin` | 3.6.2 | Tareas auxiliares |
| `org.codehaus.mojo:versions-maven-plugin` | 2.22.0 | Mantenimiento de versiones |
| `org.owasp:dependency-check-maven` | 13.0.0 | Vulnerabilidades conocidas |
| `org.jacoco:jacoco-maven-plugin` | 0.8.15 | Cobertura |

### Herramientas y propiedades auxiliares

| Componente | Propiedad | Versión | Uso |
|---|---|---|---|
| Checkstyle | `checkstyle.version` | 14.1.0 | Dependencia del plugin Checkstyle |
| Google Java Format | `google.java.format.version` | 1.36.1 | Formateador de Spotless |
| Find Security Bugs | `findsecbugs.plugin.version` | 1.14.0 | Propiedad heredable; no activa el analizador en este POM |
| UUID, propiedad conservada | `uuid.version` | 5.2.0 | Propiedad heredable sin referencias internas |

La dependencia Java UUID Generator utiliza `java-uuid-generator.version=5.2.0`. Se conservan las propiedades sin referencias internas porque los consumidores podrían utilizarlas por herencia. Dependabot no garantiza actualizar propiedades sin asociación a coordenadas Maven ni versiones incrustadas en configuraciones específicas de plugins.

Maven Site permanece en **4.0.0-M16**, la versión milestone ya utilizada por el proyecto; no existe una actualización estable de esa línea verificada en esta revisión. No se ha sustituido por la línea 3.x ni migrado Maven a una versión preliminar de la línea 4.

JaCoCo no declara ejecuciones de instrumentación o informes. Failsafe no vincula metas al ciclo de vida. Los consumidores deben activar estas funciones cuando las necesiten.

## Configuración heredada

| Propiedad | Valor |
|---|---|
| `project.build.sourceEncoding` | `UTF-8` |
| `maven.compiler.release` | `21` |
| `maven.minimum.version` | `3.9.16` |
| `checkstyle.config.location` | `google_checks.xml` |
| `checkstyle.console.output` | `true` |
| `checkstyle.fails.on.error` | `true` |
| `checkstyle.max.violations` | `0` |
| `javadoc.fail.on.error` | `true` |
| `javadoc.doclint` | `none` |
| `javadoc.quiet` | `true` |
| `dependency.check.data.directory` | `${user.home}/.m2/dependency-check-data` |
| `dependency.check.fail.build.on.cvss` | `8.0` |

Surefire y Failsafe utilizan `useModulePath=false`. SpotBugs configura esfuerzo `Max` y umbral `Medium`. Spotless utiliza Google Java Format y elimina imports no utilizados al aplicar formato. Javadoc incluye miembros privados y configura `nohelp=true`.

Dependency-Check utiliza `NVD_API_KEY`, almacena sus datos en la ruta indicada y tiene desactivado el analizador OSS Index.

Las sobrescrituras de propiedades en consumidores deben justificarse, documentarse y comprobarse.

## Construcción, calidad y seguridad

Comandos para Windows; en Linux y macOS, sustituir `.\mvnw.cmd` por `./mvnw`:

```powershell
# Validar configuración y requisitos
.\mvnw.cmd -B -ntp validate

# Ejecutar el ciclo de verificación
.\mvnw.cmd -B -ntp clean verify

# Ejecutar las comprobaciones de calidad
.\mvnw.cmd -B -ntp -Pquality clean verify

# Aplicar formato: modifica archivos Java
.\mvnw.cmd -B -ntp spotless:apply

# Ejecutar el análisis de vulnerabilidades
.\mvnw.cmd -B -ntp org.owasp:dependency-check-maven:check
```

El perfil `quality` ejecuta `spotless:check`, `checkstyle:check` y `spotbugs:check` en `verify`. Los cambios de versión de formateadores o reglas pueden producir nuevos hallazgos en los consumidores.

Dependency-Check se ejecuta por separado. Su informe se genera bajo `target`; el workflow recoge `target/dependency-check-report.*`. Un análisis del parent no sustituye el análisis de las dependencias efectivamente utilizadas por cada aplicación.

## Acceso a GitHub Packages

Cada repositorio Maven debe tener un servidor del mismo identificador en `settings.xml`.

| Identificador | URL |
|---|---|
| `github-jarios-parent` | `https://maven.pkg.github.com/contratacionmalaga/jarios-parent` |
| `github-email-helper` | `https://maven.pkg.github.com/contratacionmalaga/email_helper` |
| `github-properties-helper` | `https://maven.pkg.github.com/contratacionmalaga/properties_helper` |
| `github-version-helper` | `https://maven.pkg.github.com/contratacionmalaga/version_helper` |
| `github-encrypt-helper` | `https://maven.pkg.github.com/contratacionmalaga/encrypt_helper` |

Ejemplo mínimo para resolver el parent, que debe integrarse con la configuración Maven existente:

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0">
    <servers>
        <server>
            <id>github-jarios-parent</id>
            <username>${env.GITHUB_ACTOR}</username>
            <password>${env.PACKAGES_TOKEN}</password>
        </server>
    </servers>
    <profiles>
        <profile>
            <id>github-private-packages</id>
            <repositories>
                <repository>
                    <id>github-jarios-parent</id>
                    <url>https://maven.pkg.github.com/contratacionmalaga/jarios-parent</url>
                </repository>
            </repositories>
        </profile>
    </profiles>
    <activeProfiles>
        <activeProfile>github-private-packages</activeProfile>
    </activeProfiles>
</settings>
```

Añadir las parejas de servidor y repositorio correspondientes a los helpers utilizados. Las credenciales deben tener acceso a los paquetes y mantenerse fuera del repositorio.

`distributionManagement` define dónde publicar, no desde dónde resolver dependencias. Este POM publica mediante `github-releases` o `github-snapshots`, ambos dirigidos al repositorio de paquetes de `jarios-parent`. Los consumidores que publiquen sus propios artefactos deben revisar y, cuando corresponda, sobrescribir el destino heredado.

## GitHub Actions

| Workflow | Activación | Función |
|---|---|---|
| CI | Push y pull request a `main`; manual | `clean verify` |
| Quality | Push y pull request a `main`; manual | `-Pquality verify` |
| Dependency Check | Manual y lunes a las 04:10 UTC | Análisis OWASP y carga del informe |
| Release Package | Tags `v*`; manual con `release_tag` | Verificación, publicación y release |

Los workflows utilizan `ubuntu-latest` y Eclipse Temurin 21.

| Acción | Versión |
|---|---|
| `actions/checkout` | v7.0.1 |
| `actions/setup-java` | v6.0.1 |
| `actions/cache` | v6.1.0 |
| `actions/upload-artifact` | v7.0.1 |

Credenciales:

- `PACKAGES_TOKEN`: lectura de paquetes propios; los workflows recurren a `GITHUB_TOKEN` cuando no está definido.
- `GITHUB_TOKEN`: publicación de paquetes y creación de releases, conforme a los permisos del workflow.
- `NVD_API_KEY`: consultas de vulnerabilidades a NVD.

El token utilizado debe tener acceso efectivo a los paquetes de otros repositorios. La acción local `setup-maven-private` genera y sobrescribe `~/.m2/settings.xml` en el runner. `setup-project-artifacts` no prepara artefactos adicionales actualmente.

## Actualizaciones mediante Dependabot

La configuración reside en `.github/dependabot.yml`. GitHub ejecuta las revisiones de Dependabot y abre PR; no es necesario crear un workflow de fusión ni conceder permisos de escritura a los workflows de comprobación.

| Ecosistema | Programación | Límite de PR de versión abiertas |
|---|---|---|
| Maven | Lunes a las 07:00, `Europe/Madrid` | 10 |
| GitHub Actions | Lunes a las 07:30, `Europe/Madrid` | 5 |

Maven revisa las dependencias y plugins reconocidos en el POM, incluidas las versiones administradas mediante propiedades asociadas a artefactos. Las familias Jackson, Hibernate, Testcontainers y Surefire/Failsafe se agrupan en PR para facilitar su revisión conjunta. En GitHub Actions se agrupan actualizaciones menores y de parche; los cambios mayores se revisan por separado.

No se configura fusión automática. Las PR dirigidas a `main` activan CI y Quality. La aprobación debe incluir la revisión del diff, de los resultados y del impacto en consumidores. Los grupos no garantizan por sí solos compatibilidad ni igualdad de versiones.

### Activación y credenciales

1. Incorporar `.github/dependabot.yml` a la rama predeterminada `main` en GitHub.
2. En **Settings → Secrets and variables → Dependabot**, crear `PACKAGES_TOKEN` con un token autorizado para leer los paquetes propios. La configuración utiliza el usuario `contratacionmalaga`; adaptarlo si las credenciales pertenecen a otra cuenta.
3. Mantener también `PACKAGES_TOKEN` como secreto de Actions para las ejecuciones normales que lo necesiten. Las ejecuciones originadas por Dependabot utilizan los secretos de Dependabot, no los secretos normales de Actions.
4. Revisar las ejecuciones de actualización de Dependabot y corregir cualquier error de autenticación antes de dar por operativa la revisión de Maven.

El archivo de configuración no crea secretos. Los registros privados de Maven requieren el secreto indicado; la revisión de GitHub Actions no utiliza esos registros.

Las actualizaciones de seguridad y sus alertas dependen además de las funciones y ajustes de seguridad habilitados en GitHub. La programación de actualizaciones de versión no demuestra que esas funciones estén activas.

Documentación: [registros privados](https://docs.github.com/en/code-security/how-tos/secure-your-supply-chain/manage-your-dependency-security/configure-access-to-private-registries) y [secretos en ejecuciones de Dependabot](https://docs.github.com/en/code-security/reference/supply-chain-security/troubleshoot-dependabot/dependabot-on-actions).

### Comprobaciones complementarias

Dependabot no sustituye la revisión de propiedades sin referencias, de la distribución Maven del Wrapper, de migraciones entre coordenadas o de configuraciones específicas que no reconozca su analizador.

Para consultar versiones sin reescribir el POM:

```powershell
.\mvnw.cmd -B -ntp versions:display-dependency-updates
.\mvnw.cmd -B -ntp versions:display-plugin-updates
.\mvnw.cmd -B -ntp versions:display-property-updates
```

Estos comandos pueden descargar componentes y actualizar la caché local. Contrastar los resultados con [Maven Central](https://repo.maven.apache.org/maven2/) y las notas oficiales; las versiones preliminares o las migraciones mayores necesitan evaluación específica.

Para investigar un consumidor:

```powershell
.\mvnw.cmd help:effective-pom
.\mvnw.cmd dependency:tree
```

## Publicación de una versión

La versión se prepara localmente y solo queda disponible para consumidores remotos después de publicarse. El workflow exige un tag `v<versión>` que coincida con el POM; para esta versión corresponde **`v1.0.15`**.

El workflow obtiene el código del tag, comprueba la versión, ejecuta `clean verify`, publica con `deploy` y crea la GitHub Release si no existe. Adjunta los JAR de `target` cuando los encuentra; este proyecto publica principalmente un POM.

El workflow de release no activa el perfil `quality` ni ejecuta explícitamente Dependency-Check. Revisar esos resultados antes de publicar, actualizar las tablas de este README y validar consumidores representativos.

No se deben sobrescribir releases ya publicadas para distribuir cambios: preparar una nueva versión del parent.

## Resolución de problemas

| Síntoma | Comprobación |
|---|---|
| Parent o helper no encontrado | Versión publicada, repositorio y acceso al paquete |
| HTTP 401 o 403 | Token, permisos e identificadores de servidor/repositorio |
| Dependabot no abre PR Maven | Archivo en `main`, programación, registros y secreto de Dependabot |
| CI de Dependabot no accede a paquetes | `PACKAGES_TOKEN` en secretos de Dependabot y acceso a los paquetes |
| Fallo de Enforcer | Java y Maven mostrados por el Wrapper; revisar `JAVA_HOME` |
| Fallo de Spotless | Aplicar formato y revisar los cambios |
| Fallo de Checkstyle | Reglas de `google_checks.xml` y umbral de infracciones |
| Fallo de SpotBugs | Hallazgos y configuración del consumidor |
| Error NVD | Conectividad, clave API, límites y caché |
| Conflictos de bibliotecas | POM efectivo y árbol de dependencias |
| Fallo de Testcontainers | Docker y coherencia entre núcleo y módulos |
| Fallo de publicación | Coincidencia tag/POM, destino y permisos de escritura |

## Verificación de la versión 1.0.15

Validaciones realizadas con JDK 21.0.11 y Maven 3.9.16:

- `clean verify` con el perfil `quality` en el parent: correcto.
- Consumidor temporal del POM actualizado: compilación con Compiler 3.16.0 y dos pruebas JDBC sobre H2 2.5.252, una con Surefire 3.6.0 y otra con Failsafe 3.6.0, sin fallos.
- Formato y análisis del consumidor temporal: Spotless/Google Java Format, Checkstyle y SpotBugs completados correctamente.
- Configuración de Dependabot validada contra el esquema JSON de SchemaStore; archivos YAML analizados sin errores de sintaxis.
- Inventario del README contrastado automáticamente con las 40 dependencias y los 21 plugins del POM.

Estas comprobaciones no incluyen la ejecución remota de GitHub Actions, la publicación de paquetes, un análisis NVD ni la batería completa de los proyectos consumidores.

## Mantenimiento y licencia

Cada cambio debe describir las versiones resultantes, su impacto y las verificaciones realizadas. El inventario de esta versión no constituye una garantía de compatibilidad para todos los consumidores ni un certificado de ausencia de vulnerabilidades.

El repositorio no incluye un archivo de licencia propio ni una declaración de licencia en el POM. Las dependencias mantienen sus respectivas licencias.
