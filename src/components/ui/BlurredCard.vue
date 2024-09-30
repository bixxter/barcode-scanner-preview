<template>
    <div class="ui-blurred-card">
        <slot></slot>
    </div>
</template>

<style scoped>
.ui-blurred-card {
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
}
</style>