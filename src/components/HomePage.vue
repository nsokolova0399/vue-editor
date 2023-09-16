<template>
    <section class="home">
        <div class="home__container">
            <header class="home__header">
                <button v-on:click="moveBackward">
                    <img alt="Назад" src="./../assets/back.png">
                </button>
                <button v-on:click="moveForward">
                    <img alt="Вперед" src="./../assets/forward.png">
                </button>
                <button v-on:click="getHeading">
                    <img alt="Заголовок" src="./../assets/heading.png">
                </button>
                <button v-on:click="getParagraph">
                    <img alt="Абзац" src="./../assets/paragraph.png">
                </button>
                <button v-on:click="getImg">
                    <img alt="Вставить изображение" src="./../assets/img.png">
                </button>
                <button v-on:click="getCopy">
                    Скопировать HTML
                </button>
            </header>

            <div ref="main" class="home__main" @input="updateArray" contenteditable="true">
                Таким образом консультация с широким активом представляет собой интересный эксперимент проверки позиций,
                занимаемых участниками в отношении поставленных задач. С другой стороны постоянное
                информационно-пропагандистское обеспечение нашей деятельности представляет собой интересный эксперимент
                проверки форм развития. Идейные соображения высшего порядка, а также укрепление и развитие структуры
                влечет за собой процесс внедрения и модернизации соответствующий условий активизации. Задача
                организации, в особенности же реализация намеченных плановых заданий играет важную роль в формировании
                дальнейших направлений развития. Повседневная практика показывает, что постоянное
                информационно-пропагандистское обеспечение нашей деятельности играет важную роль в формировании
                существенных финансовых и административных условий.
                <div class="home__main_heading">Смотрите какие обезьянки</div>
                <img alt="Обезьянки" src="../assets/image.png">
                <div class="home__main_paragraph">
                    Таким образом консультация с широким активом представляет собой интересный эксперимент проверки
                    позиций, занимаемых участниками в отношении поставленных задач. С другой стороны постоянное
                    информационно-пропагандистское обеспечение нашей деятельности представляет собой инйцу шо шщйоц
                    ущойц ущошцщйуо йцщуо йщцоу щйоу шщйцош ущйтересный эксперимент проверки форм развития. Идейные
                    соображения высшего порядка, а также укрепление и развитие структуры влечет за собой процесс
                    внедрения и модернизации соответствующий условий активизации. Задача организации, в особенности же
                    реализация намеченных плановых заданий играет важную роль в формировании дальнейших направлений
                    развития. Повседневная практика показывает, что постоянное информационно-пропагандистское
                    обеспечение нашей деятельности играет важную роль в формировании существенных финансовых и
                    административных условий.
                </div>
                <div class="home__main_paragraph">
                    Товарищи! новая модель организационной деятельности требуют от нас анализа направлений
                    прогрессивного развития. Задача организации, в особенности же постоянный количественный рост и сфера
                    нашей активности требуют от нас анализа позиций, занимаемых участниками в отношении поставленных
                    задач. Задача организации, в особенности же реализация намеченных плановых заданий требуют от нас
                    анализа системы обучения кадров, соответствует насущным потребностям.
                </div>
            </div>
        </div>
    </section>
</template>

<script>
    import './homePage.css'

    export default {
        name: "HomePage",
        data() {
            return {
                selectedText: "",
                steps: [],
                currentStep: -1,
                inputValue: "",
                maxArrayLength: 20,
            }
        },
        mounted() {
            this.currentStep++;
            this.steps[this.currentStep] = this.$refs.main.outerHTML;
        },
        methods: {
            updateArray(event) {
                this.inputValue = event.target.innerText.trim();
                if (this.inputValue !== "") {
                    [this.steps, this.currentStep] = this.checkMax(this.steps, this.currentStep, this.maxArrayLength, this.$refs.main.outerHTML);
                }
            },
            checkMax(steps, currentStep, maxArrayLength, main) {
                if (currentStep < maxArrayLength - 1) {
                    currentStep++;
                    steps[currentStep] = main;
                } else {
                    steps.shift();
                    steps[currentStep] = main;
                }
                return [steps, currentStep];
            },
            moveBackward() {
                if (this.currentStep > 0) {
                    this.currentStep--;
                    this.$el.querySelector('.home__main').outerHTML = this.steps[this.currentStep];
                }
            },
            moveForward() {
                if (this.currentStep < this.steps.length - 1) {
                    this.currentStep++;
                    this.$el.querySelector('.home__main').outerHTML = this.steps[this.currentStep];
                }
            },
            getHeading() {
                this.selectedText = window.getSelection();
                if (this.selectedText) {
                    let range = this.selectedText.getRangeAt(0);
                    let selectionContents = range.extractContents();
                    let span = document.createElement("span");
                    span.appendChild(selectionContents);
                    span.setAttribute("class", "home__main_heading");
                    range.insertNode(span);
                }
                [this.steps, this.currentStep] = this.checkMax(this.steps, this.currentStep, this.maxArrayLength, this.$refs.main.outerHTML);
            },
            getParagraph() {
                this.selectedText = window.getSelection();
                if (this.selectedText) {
                    let range = this.selectedText.getRangeAt(0);
                    let selectionContents = range.extractContents();
                    let span = document.createElement("span");
                    span.appendChild(selectionContents);
                    span.setAttribute("class", "home__main_paragraph");
                    range.insertNode(span);
                }
                [this.steps, this.currentStep] = this.checkMax(this.steps, this.currentStep, this.maxArrayLength, this.$refs.main.outerHTML);
            },
            getImg() {
                const imageUrl = prompt("Введите URL изображения:");
                if (imageUrl) {
                    const img = document.createElement("img");
                    img.src = imageUrl;
                    const selection = window.getSelection();
                    if (selection.rangeCount > 0) {
                        const range = selection.getRangeAt(0);
                        range.deleteContents();
                        range.insertNode(img);
                    }
                    [this.steps, this.currentStep] = this.checkMax(this.steps, this.currentStep, this.maxArrayLength, this.$refs.main.outerHTML);
                }
            },
            getCopy() {
                navigator.clipboard.writeText(this.$el.querySelector('.home__main').outerHTML)
                document.execCommand("copy");
            },
        },
    }
</script>

<style>
</style>