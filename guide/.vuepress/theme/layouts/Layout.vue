<template>
	<div
		class="theme-container"
		:class="containerClass"
		@touchstart="onTouchStart"
		@touchend="onTouchEnd"
	>
		<Navbar v-if="shouldShowNavbar" @toggle-sidebar="toggleSidebar">
			<template #before>
				<slot name="navbar-before"></slot>
			</template>
			<template #after>
				<slot name="navbar-after"></slot>
			</template>
		</Navbar>
		<div class="content-wrapper">
			<div class="sidebar-mask" @click="toggleSidebar(false)"></div>
			<div class="sidebar-wrapper">
				<Sidebar>
					<template #top>
						<slot name="sidebar-top"></slot>
					</template>
					<template #bottom>
						<slot name="sidebar-bottom"></slot>
					</template>
				</Sidebar>
			</div>
			<Home v-if="frontmatter.home" />
			<Transition
				v-else
				name="fade-slide-y"
				mode="out-in"
				@before-enter="onBeforeEnter"
				@before-leave="onBeforeLeave"
			>
				<Page :key="page.path">
					<template #top>
						<slot name="page-top"></slot>
					</template>
					<template #bottom>
						<slot name="page-bottom"></slot>
					</template>
				</Page>
			</Transition>
		</div>
	</div>
</template>

<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref, Transition } from 'vue';
import { useRouter } from 'vue-router';
import { usePageData, usePageFrontmatter } from '@vuepress/client';
import type { DefaultThemePageFrontmatter } from '@vuepress/theme-default/lib/shared';
import Home from '@vuepress/theme-default/lib/client/components/Home.vue';
import Page from '@vuepress/theme-default/lib/client/components/Page.vue';
import { useScrollPromise, useSidebarItems, useThemeLocaleData } from '@vuepress/theme-default/lib/client/composables';
import Navbar from '../components/Navbar.vue';
import Sidebar from '../components/Sidebar.vue';

const page = usePageData();
const frontmatter = usePageFrontmatter<DefaultThemePageFrontmatter>();
const themeLocale = useThemeLocaleData();

// navbar
const shouldShowNavbar = computed(() => {
	return frontmatter.value.navbar !== false && themeLocale.value.navbar !== false;
});

// sidebar
const sidebarItems = useSidebarItems();
const isSidebarOpen = ref(false);
const toggleSidebar = (to?: boolean): void => {
	isSidebarOpen.value = typeof to === 'boolean' ? to : !isSidebarOpen.value;
};

const touchStart = { x: 0, y: 0 };
const onTouchStart = (event): void => {
	touchStart.x = event.changedTouches[0].clientX;
	touchStart.y = event.changedTouches[0].clientY;
};

const onTouchEnd = (event): void => {
	const dx = event.changedTouches[0].clientX - touchStart.x;
	const dy = event.changedTouches[0].clientY - touchStart.y;

	if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 40) {
		toggleSidebar(dx > 0 && touchStart.x <= 80);
	}
};

// classes
const containerClass = computed(() => [
	{
		'no-navbar': !shouldShowNavbar.value,
		'no-sidebar': !sidebarItems.value.length,
		'sidebar-open': isSidebarOpen.value,
	},
	frontmatter.value.pageClass,
]);

// close sidebar after navigation
let unregisterRouterHook;

onMounted(() => {
	const router = useRouter();
	unregisterRouterHook = router.afterEach(() => {
		toggleSidebar(false);
	});
});

onUnmounted(() => {
	unregisterRouterHook();
});

// handle scrollBehavior with transition
const scrollPromise = useScrollPromise();
const onBeforeEnter = scrollPromise.resolve;
const onBeforeLeave = scrollPromise.pending;
</script>
