<script lang="ts">
  import { twMerge } from 'tailwind-merge';
  import { Button } from 'flowbite-svelte';
  import { createEventDispatcher } from 'svelte';

  export let files = null;
  export let name: string = '';
  export let ref: HTMLInputElement | null = null;
  export let accept: string[] = [];
  export let multiple = false;
  export let disabled = false;
  export let size: 'xs' | 'sm' | 'md' | 'lg' | 'xl' | 'none' = 'sm';
  export let outline: boolean = false;
  export let testId: string | null = null; // for the button element

  const dispatch = createEventDispatcher();
</script>

<div>
  <input
    bind:this={ref}
    {name}
    type="file"
    accept={accept.join(',')}
    {multiple}
    bind:files
    on:click
    on:keydown={({ key }) => {
      if (key === ' ' || key === 'Enter') {
        ref?.click();
      }
    }}
    on:change={(e) => {
      dispatch('change', Array.from(e.currentTarget.files ?? []));
    }}
    {...$$restProps}
    class="sr-only"
  />
  <Button
    data-testid={testId}
    {outline}
    {size}
    {disabled}
    on:click={() => ref?.click()}
    class={twMerge('w-full', $$props.class)}
  >
    <div class="flex w-full justify-between">
      <slot />
    </div>
  </Button>
</div>
