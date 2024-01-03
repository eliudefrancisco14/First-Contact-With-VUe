<template>
    <header class="header">
        <slot name="title"/>
        
        <slot></slot>
    </header>
</template>

<script>

export default {
    name: "TheHead",
}
</script>