# Java JSON Schema Generator – Gradle Example

Minimal example show-casing how the Java JSON Schema Generator can be used within a gradle build.

----

## Usage
Depending on your OS either use the `gradlew` or `gradlew.bat` executable in the project directory.

Execute the following command:
```zsh
./gradlew -q generate
```
This will generate a schema (here for the `SchemaVersion` enum) and write it to a `schema.json` file in the project directory.
In this particular example, the schema is also printed to the console:
```
jsonschema-gradle-example % ./gradlew -q generate
{
  "$schema" : "https://json-schema.org/draft/2019-09/schema",
  "type" : "string",
  "enum" : [ "DRAFT_6", "DRAFT_7", "DRAFT_2019_09" ]
}
```

## Content of `build.gradle`
For your convenience, here is the full content of the `build.gradle` file representing this example:
```groovy
import com.github.victools.jsonschema.generator.OptionPreset;
import com.github.victools.jsonschema.generator.SchemaGenerator;
import com.github.victools.jsonschema.generator.SchemaGeneratorConfigBuilder;
import com.github.victools.jsonschema.generator.SchemaVersion;

buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath group: 'com.github.victools', name: 'jsonschema-generator', version: '4.16.0'
    }
}
plugins {
    id 'java-library'
}

task generate {
    doLast {
        def configBuilder = new SchemaGeneratorConfigBuilder(SchemaVersion.DRAFT_2019_09, OptionPreset.PLAIN_JSON);
        // apply your configurations here
        def generator = new SchemaGenerator(configBuilder.build());
        // target the class for which to generate a schema
        def jsonSchema = generator.generateSchema(SchemaVersion.class);
        // handle generated schema, e.g. write it to the console or a file
        def jsonSchemaAsString = jsonSchema.toPrettyString();
        println jsonSchemaAsString
        new File(projectDir, "schema.json").text = jsonSchemaAsString
    }
}
```
