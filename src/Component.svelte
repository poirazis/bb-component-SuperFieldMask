<script>
  import { getContext, onDestroy } from "svelte";
  import {
    SuperButton,
    SuperField,
    CellStringMask,
  } from "@poirazis/supercomponents-shared";

  // Preset mask patterns for common use cases
  const MASK_PRESETS = {
    none: null,
    phoneUS: "(000) 000-0000",
    phoneIntl: "+0 (000) 000-0000",
    ssn: "000-00-0000",
    creditCard: "0000 0000 0000 0000",
    dateUS: "00/00/0000",
    zipCode: "00000",
    time12h: "00:00 AM",
    time24h: "00:00",
  };

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

  export let field = "Masked Text Field";
  export let label;
  export let span = 6;
  export let placeholder;
  export let defaultValue;
  export let disabled;
  export let readonly;
  export let validation;
  export let helpText;
  export let align;
  export let mask;
  export let maskPreset = "none";
  export let buttons = [];

  export let onChange;
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
  $: setDefaultValue(defaultValue);

  // Determine effective mask based on preset or custom input
  $: effectiveMask =
    maskPreset === "custom" ? mask : MASK_PRESETS[maskPreset] || null;

  $: cellOptions = {
    placeholder: placeholder || (maskPreset === "none" ? field : ""),
    defaultValue,
    disabled: disabled || groupDisabled || fieldState?.disabled,
    readonly: readonly || fieldState?.readonly,
    icon,
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
      "grid-column": groupColumns ? `span ${span}` : "span 1",

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
      mask={effectiveMask}
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
            onClick={enrichButtonActions(onClick, $allContext)}
          />
        {/each}
      </div>
    {/if}
  </SuperField>
</div>
