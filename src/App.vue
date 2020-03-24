<template>
    <div id="app">
        <b-button type="info" v-on:click="preview()"
                  style="position: fixed; right: 50px; bottom: 60px">Preview
        </b-button>
        <b-button type="info" v-on:click="print()"
                  style="position: fixed; right: 150px; bottom: 60px">Download
        </b-button>
        <front-page class="a4" ref="printMe" id="printMe" :editing="editing" v-model="characterName"/>
        <back-page class="a4" id="printMe2" :editing="editing" />
    </div>
</template>

<script>
    import FrontPage from './components/FrontPage.vue'
    import BackPage from './components/BackPage.vue'
    import jsPDF from 'jspdf'
    import html2canvas from "html2canvas"

    export default {
        name: 'App',
        components: {
            FrontPage,
            BackPage,
        },
        data() {
            return {
                editing: true,
                output: null,
                characterName: '',
                pages: []
            }
        },
        methods: {
            preview() {
                this.editing = !this.editing;
            },
            async print() {
                this.editing = false;

                await setTimeout(() => {
                    // wait for rerendering
                }, 100);

                /** WITH CSS */
                await html2canvas(document.querySelector('#printMe'), {
                    scale: 2
                }).then((canvas) => {
                    this.pages.push(canvas.toDataURL('image/png'));
                });
                await html2canvas(document.querySelector('#printMe2'), {
                    scale: 2
                }).then((canvas) => {
                    this.pages.push(canvas.toDataURL('image/png'));
                });
                let pdf = new jsPDF('p', 'mm', 'a4');
                await pdf.addImage(this.pages[0], 'PNG', 0, 0, 211, 298);

                pdf.addPage();

                await pdf.addImage(this.pages[1], 'PNG', 0, 0, 211, 298);

                let fileName = this.characterName !== '' ? this.characterName : 'karta';

                pdf.save(fileName + '.pdf');
            },
        }
    }
</script>

<style lang="scss">
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px auto;
    }

    html {
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 0;
        background-color: #7a7a7a !important;
        font: 12pt "Tahoma";
    }

    * {
        box-sizing: border-box;
        -moz-box-sizing: border-box;
    }

    .a4 {
        width: 210mm;
        height: 297mm;
        padding: 5mm;
        margin: 10mm auto;
        border: 1px #D3D3D3 solid;
        border-radius: 5px;
        background: white !important;
        box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
    }

    @page {
        size: A4;
        margin: 0;
    }

    @media print {
        html, body {
            width: 210mm;
            height: 297mm;
        }
        .a4 {
            margin: 0 auto;
            border: initial;
            border-radius: initial;
            width: initial;
            min-height: initial;
            box-shadow: initial;
            background: initial;
            page-break-after: always;
        }
    }

    .columns {
        flex-wrap: wrap;
    }

    .input-box:not(.dice) {
        background: url("./assets/big-input-box.png");
        background-size: 100% 100%;
        padding: 20px;

        .field:not(:last-child) {
            margin: 0px;
        }

        .field-label {
            margin-right: 0px;
        }

        .column {
            padding: 0px;
        }

        .columns {
            height: 100%;
        }
    }

    .stat-box {
        background: url("./assets/big-input-box.png");
        background-size: 100% 100%;

        input {
            height: 20px;
            margin-top: 2px;
            margin-left: 10px;
            width: 90%;
        }

    }

    .text-box {
        padding: 25px 30px 25px 30px !important;
        background: url("./assets/textarea-box.png");
        background-size: 100% 100%;

        .input-box {
            padding: 10px;
        }

        height: 450px;

        text-align: left;
    }

    .stats-row .column {
        padding: 0px !important;
    }

    .stats {
        .column {
            margin-top: 20px;
            padding: 0px;
        }
    }

    .textarea {
        resize: none !important;
    }
</style>
