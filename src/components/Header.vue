<template>
	<header>
		<div class="header-btns header-btns-left">
			<RouterLink v-for="btn in leftBtnsData" :key="btn.id" :to="btn.to">
				{{ btn.name }}
			</RouterLink>
		</div>
		<div>
			<RouterLink to="/" class="logo-wrapper">
				<img src="../assets/logo.png" class="logo" alt="Design Mate" />
			</RouterLink>
		</div>
		<div class="header-btns header-btns-right">
			<RouterLink v-for="btn in rightBtnsData" :key="btn.id" :to="btn.to">
				{{ btn.name }}
			</RouterLink>
		</div>
	</header>
</template>

<script setup>
import { computed } from 'vue'

const btnsData = [
	{ id: 1, name: 'Главная', model: 'MainPage', to: '/' },
	{ id: 2, name: 'Галерея', model: 'GalleryPage', to: '/gallery' },
	{ id: 3, name: 'Инструкция', model: 'TutorialPage', to: '/tutorial' },
	{ id: 4, name: 'Контакты', model: 'ContactsPage', to: '/contacts' },
	{ id: 5, name: 'Регистрация', model: 'Register', to: '/register' },
	{ id: 6, name: 'Войти', model: 'Login', to: '/login' },
]

// Рассчитаем половину размера массива для деления
const halfSize = computed(() => Math.ceil(btnsData.length / 2))

// Создаем два новых вычисляемых массива для левой и правой части
const leftBtnsData = computed(() => btnsData.slice(0, halfSize.value))
const rightBtnsData = computed(() => btnsData.slice(halfSize.value))
</script>

<style scoped>
header {
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 0 30px;
	margin-top: -20px;
}
.header-btns {
	display: flex;
	flex-wrap: wrap;
	gap: 0.8em;
}
.header-btns a {
	font-weight: 700;
	padding: 0.5em;
	background-color: transparent;
	color: #301c1c;
	transition: 1s;
}
.header-btns a:hover {
	color: #883b3b;
	transition: none;
}
.header-btns-right {
	justify-content: end;
}

.logo-wrapper {
	display: block;
	width: 12em;
}
.logo {
	display: block;
	width: 100%;
	will-change: filter;
	transition: filter 300ms;
}
.logo:hover {
	filter: drop-shadow(0 0 2em #00e5ff);
}
</style>
