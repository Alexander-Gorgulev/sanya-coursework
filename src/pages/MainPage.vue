<template>
	<div class="main-page">
		<h1>
			Design<br />
			<span style="display: block; margin-left: 325px; margin-top: -40px"
				>Mate</span
			>
		</h1>
		<div class="main-page__description">
			<p>
				Онлайн-дизайнер - инновационный проект, который создает
				персонализированный интерьер в соответствии с вашим запросом. Просто
				укажите параметры комнаты и предпочтения, а программа сгенерирует
				уникальный дизайн. Это удобное решение для тех, кто хочет преобразить
				свой дом без лишних затрат времени и денег.
			</p>
			<button style="background-color: #ffe297" @click="showForm('step1')">
				Генерация
			</button>

			<Teleport to="body" v-if="currentForm">
				<div class="modal">
					<div class="modal-inner">
						<button class="modal-close" @click="closeForm">Закрыть</button>
						<form @submit.prevent="submitForm">
							<div v-if="currentForm === 'step1'">
								<h2>{{ formConfig.step1.title }}</h2>
								<template
									v-for="field in formConfig[currentForm].fields"
									:key="field.model"
								>
									<div class="input-radio">
										<input
											type="radio"
											v-model="formData[field.model]"
											:id="field.model"
											:name="field.name"
										/>
										<label :for="field.model">{{ field.label }}</label>
									</div>
								</template>
								<button @click="showForm('step2')">Далее</button>
							</div>
							<div v-else-if="currentForm === 'step2'">
								<h2>{{ formConfig.step2.title }}</h2>
								<template
									v-for="field in formConfig[currentForm].fields"
									:key="field.model"
								>
									<div>
										<input
											type="checkbox"
											v-model="formData[field.model]"
											:id="field.model"
										/>
										<label :for="field.model">{{ field.label }}</label>
									</div>
								</template>
								<button @click="showForm('step3')">Далее</button>
							</div>
							<div v-else-if="currentForm === 'step3'">
								<h2>{{ formConfig.step2.title }}</h2>
								<template
									v-for="field in formConfig[currentForm].fields"
									:key="field.model"
								>
									<div>
										<input
											type="checkbox"
											v-model="formData[field.model]"
											:id="field.model"
										/>
										<label :for="field.model">{{ field.label }}</label>
									</div>
								</template>
								<button @click="showForm('step3')">Далее</button>
							</div>
							<button v-if="currentForm === 'step3'" type="submit">
								Отправить
							</button>
						</form>
					</div>
				</div>
			</Teleport>
			<button style="background-color: #ffeec1">Купить</button>
		</div>
		<div class="main-page__img">
			<img src="../assets/ikea-sofa.png" alt="Диван, кек" />
		</div>
	</div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const formConfig = ref({
	step1: {
		id: 1,
		title: 'Используете в каких целях?',
		fields: [
			{ label: 'Дизайнер', name: 'target', type: 'radio', model: 'designer' },
			{
				label: 'Для бизнеса',
				name: 'target',
				type: 'radio',
				model: 'business',
			},
			{ label: 'Для себя', name: 'target', type: 'radio', model: 'me' },
			{ label: 'Другое', name: 'target', type: 'radio', model: 'another' },
		],
	},
	step2: {
		id: 2,
		title:
			'Какие из следующих стилей дизайна интерьера вам больше всего нравятся? ',
		fields: [
			{ label: 'Современный', type: 'checkbox', model: 'task1' },
			{ label: 'Классический', type: 'checkbox', model: 'task2' },
			{ label: 'Скандинавский', type: 'checkbox', model: 'task3' },
			{ label: 'Провансальский', type: 'checkbox', model: 'task4' },
			{ label: 'Бохо', type: 'checkbox', model: 'task5' },
			{ label: 'Минимализм', type: 'checkbox', model: 'task6' },
			{ label: 'Другое', type: 'checkbox', model: 'task7' },
		],
	},
	step3: {
		id: 3,
		title:
			'Какие из следующих стилей дизайна интерьера вам больше всего нравятся? ',
		fields: [
			{ label: 'Длина', type: 'text', model: 'length' },
			{ label: 'Ширина', type: 'text', model: 'width' },
			{ label: 'Высота', type: 'text', model: 'height' },
		],
	},
})

const currentForm = ref(null)
const formData = reactive({})

const showForm = formType => {
	currentForm.value = formType
	// Инициализация formData для новой формы
	formConfig.value[formType].fields.forEach(field => {
		formData[field.model] = field.type === 'checkbox' ? false : ''
	})
}

const closeForm = () => {
	currentForm.value = null
}

const submitForm = () => {
	console.log('Форма отправлена с данными:', formData)
	closeForm()
}
</script>

<style scoped>
h1 {
	color: #301c1c;
	font-weight: 700;
	font-size: 120px;
	margin-bottom: 0;
	margin-top: 0;
}
.main-page {
	padding: 0 150px;
}
.main-page__description {
	display: flex;
	align-items: center;
	gap: 30px;
}
.main-page__description p {
	font-weight: 300;
	margin-right: 40px;
	font-size: 14px;
	max-width: 45%;
	color: #1e1e1a;
}
.main-page__description button {
	height: 64px;
	padding: 0 40px;
	font-size: 20px;
	color: #1e1e1a;
	border-radius: 30px;
}
.main-page__img {
	margin: 40px auto 0;
	width: 755px;
}
.main-page__img img {
	width: 100%;
}

.modal {
	position: fixed;
	left: 0;
	right: 0;
	top: 0;
	bottom: 0;
	display: flex;
	align-items: center;
	justify-content: center;
	background-color: #d2c5a2;
}
.modal-inner {
	background-color: #fdffaf;
	padding: 30px 100px 40px;
	border-radius: 40px;
	color: #000;
}
.modal-close {
	display: block;
	margin-left: auto;
	margin-right: -60px;
}

.input-radio {
	margin-bottom: 20px;
}
.input-radio input[type='radio'] {
	display: none;
}
</style>
