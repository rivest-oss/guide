<template>
	<div class="theme-options">
		<ul class="color-theme-options">
			<span>Tema</span>
			<li>
				<button
					class="default-theme"
					title="Verde"
					@click.prevent="setTheme({ persist: true })"
				></button>
			</li>
			<li>
				<button
					class="blurple-theme"
					title="Blurple"
					@click.prevent="setTheme({ colorTheme: 'blurple', persist: true })"
				></button>
			</li>
			<li>
				<button
					class="orange-theme"
					title="Orange"
					@click.prevent="setTheme({ colorTheme: 'orange', persist: true })"
				></button>
			</li>
			<li>
				<button
					class="purple-theme"
					title="Purple"
					@click.prevent="setTheme({ colorTheme: 'purple', persist: true })"
				></button>
			</li>
		</ul>
	</div>
</template>

<script setup lang="ts">
import useColorTheme from '../composables/useColorTheme';

const { setTheme } = useColorTheme();
</script>

<style lang="scss">
.theme-options {
	.color-theme-options {
		display: flex;

		> span {
			margin-right: auto;
		}

		li {
			margin-left: 0.5rem;

			button {
				color: #fff;
				display: block;
				width: 1.25rem;
				height: 1.25rem;
				border: 0;
				border-radius: 2px;
				cursor: pointer;

				&.default-theme {
					background-color: var(--green-500);

					&:hover {
						background-color: var(--green-530)
					}
				}

				&.blurple-theme {
					background-color: var(--blurple-500);

					&:hover {
						background-color: var(--blurple-530)
					}
				}

				&.orange-theme {
					background-color: var(--orange-500);

					&:hover {
						background-color: var(--orange-530)
					}
				}

				&.purple-theme {
					background-color: var(--purple-500);

					&:hover {
						background-color: var(--purple-530)
					}
				}
			}
		}
	}
}
</style>
