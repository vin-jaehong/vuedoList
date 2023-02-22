<template>
    <div class="container">
        <slot></slot>
    </div>
</template>

<script setup>
    
</script>

<style>
    .container {
        position: relative;
        margin: 0 auto;
        width: 1400px;
        text-align: center;
        padding: 5% 5% 0 5%;
    }
    @media (max-width:1420px) {
        .container {
            width: 100%;
            padding: 5% 0 0 0;
        }
    }
</style>