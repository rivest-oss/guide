<template>
	<div class="toggle-sidebar-button" @click="$emit('toggle')">
		<MenuIcon class="icon sidebar-menu-icon" />
		<CloseIcon class="icon sidebar-close-icon" />
	</div>
</template>

<script setup lang="ts">
import MenuIcon from './icons/Bars.vue';
import CloseIcon from './icons/Close.vue';

// NOTE: May need to be renamed to `defineEmits` in a later Vue 3.x version
defineEmits(['toggle']);
</script>
