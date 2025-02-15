[//]: # (title: Pebble)
[pebble_engine_builder]: https://pebbletemplates.io/com/mitchellbosecke/pebble/PebbleEngine/Builder/

<microformat>
<var name="example_name" value="pebble"/>
<include src="lib.xml" include-id="download_example"/>
</microformat>

Ktor allows you to use [Pebble templates](https://pebbletemplates.io/) as views within your application by installing the [Pebble](https://api.ktor.io/ktor-features/ktor-pebble/ktor-pebble/io.ktor.pebble/-pebble/index.html) plugin (previously known as feature).


## Add dependencies {id="add_dependencies"}
<var name="feature_name" value="Pebble"/>
<var name="artifact_name" value="ktor-pebble"/>
<include src="lib.xml" include-id="add_ktor_artifact_intro"/>
<include src="lib.xml" include-id="add_ktor_artifact"/>

## Install Pebble {id="install_feature"}

<var name="feature_name" value="Pebble"/>
<include src="lib.xml" include-id="install_feature"/>

Inside the `install` block, you can [configure](#configure) the [PebbleEngine.Builder][pebble_engine_builder] for loading Pebble templates.


## Configure Pebble {id="configure"}
### Configure template loading {id="template_loading"}
To load templates, you need to configure how to load templates using [PebbleEngine.Builder][pebble_engine_builder]. For example, the code snippet below enables Ktor to look up templates in the `templates` package relative to the current classpath:

```kotlin
```
{src="snippets/pebble/src/main/kotlin/com/example/Application.kt" lines="12-16"}

### Send a template in response {id="use_template"}
Imagine you have the `index.html` template in `resources/templates`:

```html
```
{src="snippets/pebble/src/main/resources/templates/index.html"}

A data model for a user looks as follows:

```kotlin
```
{src="snippets/pebble/src/main/kotlin/com/example/Application.kt" lines="25"}

To use the template for the specified [route](Routing_in_Ktor.md), pass `PebbleContent` to the `call.respond` method in the following way:

```kotlin
```
{src="snippets/pebble/src/main/kotlin/com/example/Application.kt" lines="18-21"}
