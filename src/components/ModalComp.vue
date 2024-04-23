<template>
	<div>
		<div class="modal">
			<div class="modal-inner" :class="{ step3Class: isActiveModal }">
				<RouterLink class="modal-close" to="/">Закрыть</RouterLink>
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
						<button @click="showForm('step2')" class="btn-next">Далее</button>
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
						<button @click="showForm('step3')" class="btn-next">Далее</button>
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
					</div>
					<div v-else-if="currentForm === null">
						<h2>Благодарим за информацию!</h2>
            <h4>Используемые стили:</h4>
						<p>
							{{ getLabelsForModels(extractActiveCategories(formData), formConfig.step2.fields) }}
						</p>
					</div>
					<button
						v-if="currentForm === 'step3'"
						type="submit"
						class="btn-next"
						style="display: block"
            @click="loadRandomUrl(extractActiveCategories(formData))"
					>
						Отправить
					</button>
				</form>
			</div>
			<div class="modal-view modal-inner" v-if="isActiveModal">
				<img
					:src="selectedText"
				/>
			</div>
		</div>
	</div>
</template>

<script setup>
import {ref, reactive, defineProps, onMounted} from 'vue'
import axios from 'axios';

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
			{ label: 'Современный', type: 'checkbox', model: 'modern' },
			{ label: 'Классический', type: 'checkbox', model: 'classical' },
			{ label: 'Скандинавский', type: 'checkbox', model: 'scandinavia' },
			{ label: 'Провансальский', type: 'checkbox', model: 'provencal' },
			{ label: 'Бохо', type: 'checkbox', model: 'boho' },
			{ label: 'Минимализм', type: 'checkbox', model: 'minimalism' },
			{ label: 'Другое', type: 'checkbox', model: 'other' },
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

const isActiveModal = ref(false)
const currentForm = ref('step1')
const formData = reactive({})

const showForm = formType => {
	currentForm.value = formType
	formConfig.value[formType].fields.forEach(field => {
		formData[field.model] = field.type === 'checkbox' ? false : ''
	})
}

const closeForm = () => {
	currentForm.value = null
}

const submitForm = () => {
	isActiveModal.value = true
	console.log('Форма отправлена с данными:', formData)
	closeForm()
}

const selectedCategory = ref(null);
const selectedText = ref(null);

async function loadRandomUrl(categories) {
  try {
    console.log('categories:',categories);
    const response = await axios.get('/example/data.json'); // Укажите правильный путь к файлу
    const data = response.data;
    const filteredCategories = categories.filter(category => category in data);
    console.log("filteredCategories: ",filteredCategories);
    if (filteredCategories.length === 0) {
      console.error("None of the specified categories exist in the data.");
      return;
    }
    const randomCategory = filteredCategories[Math.floor(Math.random() * filteredCategories.length)];
    const texts = data[randomCategory];
    const randomText = texts[Math.floor(Math.random() * texts.length)];
    selectedCategory.value = randomCategory;
    selectedText.value = randomText;
  } catch (error) {
    console.error('Failed to load data:', error);
  }
}

function extractActiveCategories(formData) {
  // Создаем массив для хранения активных категорий
  let activeCategories = [];

  // Проходим по всем ключам в объекте formData
  for (let key in formData) {
    // Проверяем, равно ли значение ключа true
    if (formData[key] === true) {
      // Если да, добавляем ключ в массив активных категорий
      activeCategories.push(key);
    }
  }

  // Возвращаем массив активных категорий
  return activeCategories;
}

function getLabelsForModels(models, fields) {
  // Используем map для создания нового массива, содержащего метки для каждой модели
  let labels = models.map(model => {
    // Находим элемент в fields, где model совпадает с текущим элементом models
    const field = fields.find(field => field.model === model);
    // Возвращаем label, если элемент найден, иначе null
    return field ? field.label : null;
  });

  // Фильтруем null значения (на случай, если какие-то модели не были найдены)
  labels = labels.filter(label => label !== null);

  // Преобразуем массив меток в строку, разделенную запятыми
  return labels.join(', ');
}

// onMounted(async () => {
//   loadRandomUrl(extractActiveCategories(formData))
// })

</script>

<style scoped>
/* Modal`ка */
.modal {
	position: fixed;
	left: 0;
	right: 0;
	top: 0;
	bottom: 0;
	display: flex;
	padding: 20px;
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
	width: max-content;
	display: block;
	margin-left: auto;
	border-radius: 8px;
	border: 1px solid transparent;
	padding: 0.6em 1.2em;
	font-size: 0.8em;
	background-color: #e4e4e4;
	color: #000;
	font-weight: 600;
}
.modal-close:hover {
	transition: 0.3s ease;
	background-color: #e4e4e48a;
}
.btn-next {
	margin-top: 20px;
}

/* input`ы */
.input-radio {
	display: flex;
	align-items: center;
	font-family: sans-serif;
	margin-bottom: 10px;
}

.input-radio input[type='radio'] {
	appearance: none;
	-webkit-appearance: none;
	position: relative;
	margin-left: -2px;
}

.input-radio label {
	cursor: pointer;
	position: relative;
	padding-left: 30px; /* space for the custom radio */
}

.input-radio input[type='radio'] + label::before {
	content: '';
	position: absolute;
	left: 0;
	top: 50%;
	transform: translateY(-50%);
	width: 16px;
	height: 16px;
	border: 2px solid orange;
	border-radius: 50%;
	background-color: transparent;
}

.input-radio input[type='radio']:checked + label::after {
	content: '';
	position: absolute;
	left: 6px; /* Center the inner circle */
	top: 50%;
	transform: translateY(-50%);
	width: 8px;
	height: 8px;
	background-color: orange;
	border-radius: 50%;
}
/* STEP 3 STYLES */
.step3Class {
	width: 30%;
}

.modal-view {
	margin-left: 80px;
	width: 70%;
	height: 90%;
	padding: 30px;
}
.modal-view img {
	object-fit: cover;
	border-radius: 20px;
	width: 100%;
	height: 100%;
}
</style>
