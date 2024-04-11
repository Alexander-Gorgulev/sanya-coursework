<template>
	<div class="wrapper">
		<div class="wrapper-in">
			<Header @change-page="changePage"></Header>
			<div class="container">
				<transition name="fade" mode="out-in">
					<component :is="currentPage" />
				</transition>
			</div>
		</div>
	</div>
</template>

<script setup>
import { markRaw, shallowRef } from 'vue'
import Header from './components/Header.vue'
import MainPage from './pages/MainPage.vue'
import Register from './pages/Register.vue'
import NotFound from './components/NotFound.vue'

// Словарь компонентов, помеченных как raw
const pages = {
	MainPage: markRaw(MainPage),
	Register: markRaw(Register),
	NotFound: markRaw(NotFound),
	// Другие страницы
}

const currentPage = shallowRef(pages.MainPage) // Используем shallowRef для текущей страницы

// Функция для смены страницы
const changePage = pageName => {
	if (pages[pageName]) {
		currentPage.value = pages[pageName]
	} else {
		currentPage.value = pages.NotFound
	}
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
	transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
	opacity: 0;
}
.container {
	padding: 0 30px;
}
.wrapper {
	min-height: 100vh;
}
</style>
