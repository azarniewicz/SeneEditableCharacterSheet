<template>
    <div id="app">
        <b-upload v-model="file" style="display:none" >
            <a class="button"  ref="uploadButton">
                <b-icon icon="upload"></b-icon>
                <span>Click to upload</span>
            </a>
        </b-upload>
        <fab :actions="fabActions"
             :bg-color="'#278be3'"
             @print="print"
             @preview="preview"
             @exportData="exportData"
             @importData="importData"
        ></fab>
        <b-loading :is-full-page="true" :active.sync="isLoading"></b-loading>
        <front-page class="a4" ref="printMe" id="printMe" :editing="editing" v-model="frontPageData"/>
        <back-page class="a4" id="printMe2" :editing="editing" v-model="backPageData"/>
    </div>
</template>

<script>
    import jsPDF from 'jspdf'
    import html2canvas from "html2canvas"
    import fab from 'vue-fab'
    import FrontPage from './components/FrontPage.vue'
    import BackPage from './components/BackPage.vue'

    export default {
        name: 'App',
        components: {
            FrontPage,
            BackPage,
            fab
        },
        data() {
            return {
                editing: true,
                output: null,
                characterName: '',
                pages: [],
                frontPageData: {},
                backPageData: {},
                file: null,
                isLoading: false,
                fabActions: [
                    {
                        name: 'preview',
                        icon: 'remove_red_eye',
                        tooltip: 'Podgląd'
                    },
                    {
                        name: 'print',
                        icon: 'picture_as_pdf',
                        tooltip: 'Pobierz PDF'
                    },
                    {
                        name: 'exportData',
                        icon: 'get_app',
                        tooltip: 'Eksportuj do pliku JSON'
                    },
                    {
                        name: 'importData',
                        icon: 'publish',
                        tooltip: 'Importuj plik JSON'
                    },
                ]
            }
        },
        methods: {
            preview() {
                this.editing = !this.editing;
            },
            async print() {
                this.editing = false;
                this.isLoading = true;

                await setTimeout(() => {
                    // wait for rerendering
                }, 100);

                /** WITH CSS */
                await html2canvas(document.querySelector('#printMe'), {
                    scale: 1.2
                }).then((canvas) => {
                    this.pages.push(canvas.toDataURL('image/png'));
                });
                await html2canvas(document.querySelector('#printMe2'), {
                    scale: 1.2
                }).then((canvas) => {
                    this.pages.push(canvas.toDataURL('image/png'));
                });
                let pdf = new jsPDF('p', 'mm', 'a4');
                await pdf.addImage(this.pages[0], 'PNG', 0, 0, 211, 298);

                pdf.addPage();

                await pdf.addImage(this.pages[1], 'PNG', 0, 0, 211, 298);

                let fileName = this.frontPageData.imie !== '' ? this.frontPageData.imie : 'karta';

                this.isLoading = false;

                pdf.save(fileName + '.pdf');
            },
            saveText(text, filename){
                let a = document.createElement('a');
                a.setAttribute('href', 'data:text/plain;charset=utf-8,'+encodeURIComponent(text));
                a.setAttribute('download', filename);
                a.click()
            },
            importData() {
                this.$refs.uploadButton.click();
            },
            exportData() {
                let data = JSON.stringify({
                    front: this.frontPageData,
                    back: this.backPageData
                });
                let name = this.frontPageData.imie !== '' ? this.frontPageData.imie + '.json' : 'export.json';
                this.saveText(data, name);
            }
        },
        watch: {
            file(val) {
                const reader = new FileReader();
                reader.readAsText(val, "UTF-8");
                reader.onload = (evt) => {
                    let data = JSON.parse(JSON.parse(JSON.stringify(evt.target.result)));
                    this.frontPageData = data.front;
                    this.backPageData = data.back;
                };
            }
        }
    }
</script>

<style lang="scss">
    @import "~bulma/sass/utilities/_all";

    $tablet: '100px';
    // Import Bulma and Buefy styles
    @import "~bulma";
    @import "~buefy/src/scss/buefy";

    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px auto;
        overflow: auto;
        overflow: -moz-scrollbars-none;
        -ms-overflow-style: none;
        position: relative;
        will-change: scroll-position !important;
        max-height: 100vh;
    }

    #app::-webkit-scrollbar { width: 0 !important }

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

    * {
        will-change: scroll-position !important;
    }
</style>
