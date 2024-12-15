# API - `$e.hooks`

The `$e.hooks` api is a manager of `$e.hooks.ui` and `$e.hooks.data`. It allow you to create custom hooks. The hooks attached to `$e.commands` and each hook fired _after/before_ running a command, that runs by `$e.run()`.

**Location**: `core/common/assets/js/api/core/hooks.js`

## Methods

| Method                              | Params                                                                                         |  Returns              | Description                                                         |
|-------------------------------------|------------------------------------------------------------------------------------------------|-----------------------|---------------------------------------------------------------------|
| `$e.hooks.activate()`               |                                                                                                |                       | Activate all hooks.                                                 |
| `$e.hooks.deactivate()`             |                                                                                                |                       | Deactivate all hooks.                                               |
| `$e.hooks.getAll()`                 |                                                                                                | `{Array}`             | Receive all loaded hooks.                                           |
| `$e.hooks.register()`               | `{String}` *type*, `{String}` *event*, `{HookBase}` *instance*                                 | `{Object}` *callback* | Register a hook.                                                    |
| `$e.hooks.run()`                    | `{String}` *type*, `{String}` *event*, `{String}` *command*, `{Object}` *args*, `{*}` *result* | `{Boolean}`           | Run a hook.                                                         |
| `$e.hooks.registerDataAfter()`      | `{HookBase}` *instance*                                                                        | `{Object}` *callback* | Register data hook that runs after the command runs.                |
| `$e.hooks.registerDataCatch()`      | `{HookBase}` *instance*                                                                        | `{Object}` *callback* | Register data hook that runs when the command fails.                |
| `$e.hooks.registerDataDependency()` | `{HookBase}` *instance*                                                                        | `{Object}` *callback* | Register data hook that runs before the command runs as dependency. |
| `$e.hooks.registerUIAfter()`        | `{HookBase}` *instance*                                                                        | `{Object}` *callback* | Register UI hook that runs after the commands run.                  |
| `$e.hooks.registerUICatch()`        | `{HookBase}` *instance*                                                                        | `{Object}` *callback* | Register UI hook that runs when the command fails.                  |
| `$e.hooks.registerUIBefore()`       | `{HookBase}` *instance*                                                                        | `{Object}` *callback* | Register UI hook that runs before the command.                      |
| `$e.hooks.runDataAfter()`           | `{String}` *command*, `{Object}` *args*, `{*}` *result*                                        | `{Boolean}`           | Run a data hook that runs after the command.                        |
| `$e.hooks.runDataCatch()`           | `{String}` *command*, `{Object}` *args*, `{*}` *result*                                        | `{Boolean}`           | Run a data hook that runs when the command fails.                   |
| `$e.hooks.runDataDependency()`      | `{String}` *command*, `{Object}` *args*, `{*}` *result*                                        | `{Boolean}`           | Run a data hook that runs before the command as dependency.         |
| `$e.hooks.runUIAfter()`             | `{String}` *command*, `{Object}` *args*, `{*}` *result*                                        | `{Boolean}`           | Run a UI hook that runs after the commands run.                     |
| `$e.hooks.runUICatch()`             | `{String}` *command*, `{Object}` *args*, `{*}` *result*                                        | `{Boolean}`           | Run a UI hook that runs when the command fails.                     |
| `$e.hooks.runUIBefore()`            | `{String}` *command*, `{Object}` *args*, `{*}` *result*                                        | `{Boolean}`           | Run a UI hook that runs before the command.                         |

## Guidelines

* Each hook is owned by a [component](./components/).

* Each [component](./components/), can extend `defaultHooks` method which are used to import the hooks.

* The hooks imported via built-in method called `importHooks`.
  * All the hooks should be exported in one index file:

```javascript
// index.js
export { FooterSaverRefreshMenu } from './ui/document/elements/settings/footer-saver-refresh-menu';
export { UpdateButton } from './ui/document/save/set-is-modifed/update-button';
export { BypassImport } from './data/document/elements/import/bypass-import';
export { SaveExtras } from './data/document/save/save/save-extras';
```

You can have as many indexes in every hierarchy under `component/hooks/what-ever-you-wish` as you wish to organize your code, the requirement is to have one index file,
at `component/hooks/index.js` which exports all the hooks, take the **index.js** example above as a scenario:

```
📦 component
│   📜 component.js
│
└───📂 hooks
│   │   📜 index.js ( has all the hooks exported )
│   │
│   └───📂 ui
│   │   └───📂 document
│   │   │   └───📂 elements
│   │   │   │   └───📂 settings
│   │   │   │   │   │   📜 footer-saver-refresh-menu.js
│   │   │   │   │   │   ...
│   │   │   └───📂 save
│   │   │   │   └───📂 set-is-modfifed
│   │   │   │   │   │   📜 update-button.js
│   │   │   │   │   │   ...
│   │   📜 index.js ( has all ui hooks exported )
│   │   ...
│   └──📂 data
│   │   └───📂 document
│   │   │   └───📂 elements
│   │   │   │   └───📂 import
│   │   │   │   │   │   📜 bypass-import.js
│   │   │   │   │   │   ...
│   │   │   └───📂 save
│   │   │   │   └───📂 save
│   │   │   │   │   │   📜 save-extras.js
│   │   │   │   │   │   ...
│   │   📜 index.js ( has all data hooks exported )
│   │   ...
```

`component/hooks/index.js` file at line _5_:

```javascript
export * from './ui/';
export * from './data/';
```

`component/hooks/ui/index.js` file at line _17_:

```javascript
export { FooterSeverRefreshMenu } from './document/elements/settings/footer-saver-refresh-menu';
export { UpdateButton } from './document/save/set-is-modifed/update-button';
```

`component/hooks/data/index.js` file at line _29_:

```javascript
export { BypassImport } from './document/elements/import/bypass-import';
export { SaveExtras } from './document/save/save/save-extras';
```

Use `importHooks` example: `component/component.js` file at line _2_:

```javascript
import * as hooks from './hooks/';

export class Component extends $e.modules.ComponentBase {
    getNamespace() {
        return 'component-name';
    }

    defaultHooks() {
        return this.importHooks( hooks );
    }
}
```

## Conventions

```javascript
// {FILE_PATH}/{FILE_NAME} - This is line should be deleted - just for the exmaple.
import HookUIAfter from 'elementor-api/modules/hooks/{TYPE}/after';

export class {FILE_NAME_CAMEL_CASE} extends HookUIAfter {
	getCommand() {
		return '{COMMAND}';
	}

	getId() {
		return '{FILE_NAME_WITHOUT_JS}--{COMMAND}';
	}

	getContainerType() {
		return '{CONTAINER_TYPE}';
	}

	getConditions( args ) {
		return args.settings && 'undefined' !== typeof args.settings.post_status;
	}

	apply( args ) {
		const { footerSaver } = $e.components.get( 'document/save' );

		footerSaver.setMenuItems( args.container.document );

		footerSaver.refreshWpPreview();
	}
}

export default {FILE_NAME_CAMEL_CASE};
```

### Legend

| Name                   | Format  - Description               | Value for example. 
|------------------------|-------------------------------------|---------------------
|`{TYPE}`                | `ui` or `data` depends on the hook  | `ui`
|`{COMMAND}`             | which command to hook               | `document/elements/settings`
|`{FILE_NAME}`           | kebab case, name is description oh what the hook does | `footer-saver-refresh-menu.js`
|`{FILE_NAME_CAMEL_CASE}`| camel case, of `{FILE_NAME}`        | `FooterSaverRefreshMenu`
|`{FILE_NAME_WITHOUT_JS}`| `{FILE_NAME}` without `.js`         | `footer-saver-refresh-menu`
|`{FILE_PATH}`           | `{TYPE}/{COMMAND}/{FILE_NAME}`      | `ui/document/elements/settings/footer-saver-refresh-menu.js`
|`{CONTAINER_TYPE}`      | optional, gain performance if container type is known in advance | `document`

### Example

```javascript
// ui/document/elements/settings/footer-saver-refresh-menu.js - This line should be deleted - just for the example.
import HookUIAfter from 'elementor-api/modules/hooks/ui/after';

export class FooterSaverRefreshMenu extends HookUIAfter {
	getCommand() {
		return 'document/elements/settings';
	}

	getId() {
		return 'footer-save-refresh-menu--document/elements/settings';
	}

	getContainerType() {
		return 'document';
	}

	getConditions( args ) {
		return args.settings && 'undefined' !== typeof args.settings.post_status;
	}

	apply( args ) {
		const { footerSaver } = $e.components.get( 'document/save' );

		footerSaver.setMenuItems( args.container.document );

		footerSaver.refreshWpPreview();
	}
}

export default FooterSaverRefreshMenu; // Required - comment should be deleted.
```

::: warning Note
Further information about how to use hooks, can be found at [`{$e.hooks.ui}`](./hooks/ui/) and [`{$e.hooks.data}`](./hooks/data/) accoridng to their type.
:::
