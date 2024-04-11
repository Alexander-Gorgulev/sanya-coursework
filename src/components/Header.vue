<template>
	<header>
		<div class="header-btns header-btns-left">
			<button
				v-for="btn in leftBtnsData"
				:key="btn.id"
				@click="$emit('change-page', btn.model)"
			>
				{{ btn.name }}
			</button>
		</div>
		<div>
			<a href="/" class="logo-wrapper">
				<img src="../assets/logo.png" class="logo" alt="Design Mate" />
			</a>
		</div>
		<div class="header-btns header-btns-right">
			<button
				v-for="btn in rightBtnsData"
				:key="btn.id"
				@click="$emit('change-page', btn.model)"
			>
				{{ btn.name }}
			</button>
		</div>
	</header>
</template>

<script setup>
import { computed } from 'vue'

const btnsData = [
	{ id: 1, name: 'Главная', model: 'MainPage' },
	{ id: 2, name: 'Галерея', model: 'GalleryPage' },
	{ id: 3, name: 'Инструкция', model: 'TutorialPage' },
	{ id: 4, name: 'Контакты', model: 'ContactsPage' },
	{ id: 5, name: 'Регистрация', model: 'Register' },
	{ id: 6, name: 'Войти', model: 'Login' },
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
.header-btns button {
	font-weight: 700;
	padding: 0.5em;
	background-color: transparent;
	color: #301c1c;
	transition: 1s;
}
.header-btns button:hover {
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
