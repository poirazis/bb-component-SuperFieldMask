<script>
  import { getContext, onDestroy } from "svelte";
  import {
    SuperButton,
    SuperField,
    CellStringMask,
  } from "@poirazis/supercomponents-shared";

  const { styleable, enrichButtonActions, Provider, builderStore } =
    getContext("sdk");
  const component = getContext("component");
  const allContext = getContext("context");

  const form = getContext("form");
  const formStepContext = getContext("form-step");
  const groupLabelPosition = getContext("field-group");
  const labelWidth = getContext("field-group-label-width");
  const groupColumns = getContext("field-group-columns");
  const groupDisabled = getContext("field-group-disabled");

  export let field = "Text Field";
  export let label;
  export let span = 6;
  export let placeholder;
  export let defaultValue;
  export let template;
  export let disabled;
  export let readonly;
  export let validation;
  export let helpText;
  export let align;
  export let mask;
  export let buttons = [];
  export let inFieldGroup;

  export let onChange;
  export let debounced;
  export let debounceDelay;
  export let autofocus;
  export let invisible = false;

  export let icon;

  export let role = "formInput";
  export let labelPosition = "fieldGroup";
  export let showDirty;

  let formField;
  let formStep;
  let fieldState;
  let fieldApi;
  let fieldSchema;
  let value = defaultValue;
  let unsubscribe;

  $: setDefaultValue(defaultValue);

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
    fieldSchema = value?.fieldSchema;
  });

  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: labelPos =
    groupLabelPosition && labelPosition == "fieldGroup"
      ? groupLabelPosition
      : labelPosition;

  $: formField = form?.formApi.registerField(
    field,
    "string",
    defaultValue,
    disabled,
    readonly,
    validation,
    formStep
  );

  $: error = fieldState?.error;
  $: value = fieldState?.value;

  $: _inFieldGroup = groupColumns ? true : false;

  $: if (
    _inFieldGroup !== inFieldGroup &&
    $component.selected &&
    $builderStore.inBuilder
  ) {
    builderStore.actions.updateProp("inFieldGroup", _inFieldGroup);
  }

  $: cellOptions = {
    placeholder: placeholder || field,
    defaultValue,
    disabled: disabled || groupDisabled || fieldState?.disabled,
    template,
    readonly: readonly || fieldState?.readonly,
    icon,
    debounce: debounced ? debounceDelay : false,
    align,
    error: fieldState?.error,
    role,
    showDirty,
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      display:
        invisible && !$builderStore.inBuilder
          ? "none"
          : $component.styles.normal.display,
      opacity: invisible && $builderStore.inBuilder ? 0.6 : 1,
      "grid-column": span < 7 ? "span " + span : "span " + groupColumns * 6,

      flex: span > 6 ? "auto" : "none",
    },
  };

  const handleChange = (newValue) => {
    if (!form) value = newValue;
    fieldApi?.setValue(newValue);
    onChange?.({ value: newValue });
  };

  const setDefaultValue = (val) => {
    value = val;
  };

  onDestroy(() => {
    fieldApi?.deregister();
    unsubscribe?.();
  });
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div use:styleable={$component.styles} class:invisible>
  <Provider data={{ value }} />
  <SuperField {labelPos} {labelWidth} {field} {label} {error} {helpText}>
    <CellStringMask
      {cellOptions}
      {value}
      {fieldSchema}
      {autofocus}
      mask={mask}
      on:change={(e) => handleChange(e.detail)}
    />
    {#if buttons?.length}
      <div class="inline-buttons">
        {#each buttons as { text, onClick, quiet, type, size, icon }}
          <SuperButton
            {quiet}
            {disabled}
            {size}
            {type}
            {text}
            {icon}
            on:click={enrichButtonActions(onClick, $allContext)}
          />
        {/each}
      </div>
    {/if}
  </SuperField>
</div>
