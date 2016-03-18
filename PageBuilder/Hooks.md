# Blue Foot: Page Builder
## Hooks

The hook system is a pub/sub, observer/event system built directly into the core of the page builder. This enables easy extension to numerous actions a user can make whilst interacting with the page builder. 

You can find the hook JavaScript at `js/gene/cms/component/core/hook.js` this is the first thing that is initialized in the page builder application. It's also heavily used by the core for updating various UI elements.

#### Triggering an event
If you have created a widget, or custom plugin you may need the system to know you've updated something to re-trigger previews or rendering. You can do this with the following code:

```
Hook.trigger('gene-bluefoot-stage-ui-updated', false, false, this.stage);
```
When calling `Hook.trigger` there are 4 possible paramters, the first is already required.

- **name**: The event you wish to trigger.
- **params**: Any parameters you wish to be available to anything attached onto this event.
- **completeFn**: A function to be ran once the event has been fully ran.
- **origin**: The origin of the hook being trigger, it's good practice to pass the current stage into this parameter as the hook system is global.

#### Attaching/observing an event
If you're wanting to conduct a certain action when the system triggers a certain event you can declare an attachment onto that events name. For instance the system uses these to tweak various parts of the UI when entities are updated.

```
Hook.attach('gene-bluefoot-stage-ui-updated', function ($hook) {
    // Logic here
    $hook.done();
}.bind(this));
```
Attaching onto an event only has two parameters, the first being the name of the event you're wanting to attach onto and the second being the function to call when the event is triggered.

The complete function will be passed an instance of `$hook` for the system to behave correctly you must **always** call `$hook.done();` once your processing is finished, failure to do so could cause some very strange behaviour.

#### Parameters
When parameters are passed during the trigger event they're available within the `$hook` object, under the `params` key.

```
Hook.trigger('example-hook', {message: 'hello'}, false, this.stage);
```

You could then access the message variable via
```
$hook.params.message;
```

#### Core hooks
The core implements a number of different hooks to allow your plugin / widget to interact with many different parts of Blue Foot.

- gene-bluefoot-stage-ui-updated
- gene-bluefoot-stage-restore-empty-text
- gene-bluefoot-stage-updated
- gene-bluefoot-stage-visible
- gene-bluefoot-entity-update-previews
- gene-bluefoot-after-edit-view
- gene-bluefoot-after-edit-init
- gene-bluefoot-save-configure-entity
- gene-bluefoot-validate-form
- gene-bluefoot-before-render-field
- gene-bluefoot-after-render-field-*FIELD_TYPE*
- gene-bluefoot-plugins-prepare-before
- gene-bluefoot-plugins-prepare-paths
- gene-bluefoot-plugins-prepare-after
- gene-bluefoot-before-stage-init
- gene-bluefoot-after-stage-init
- gene-bluefoot-build-complete
- gene-bluefoot-panel-get-config
- gene-bluefoot-stage-save-extra
- gene-bluefoot-get-options-*CODE*

> If you believe we should implement further hooks throughout the system please contact us and we will include these in future releases.