<template>
    <div>
        <div class="columns has-text-left">
            <div class="column is-two-fifths">
                <div class="input-box">
                    <div class="columns">
                        <div class="column">
                            <b-field horizontal
                                     label="Imię:"
                                     custom-class="is-small"
                                     v-if="editing"
                            >
                                <b-input v-model="imie" custom-class="is-small" expanded></b-input>
                            </b-field>
                            <span v-else>
                                Imię: {{ imie }}
                            </span>
                            <b-field horizontal
                                     label="Rasa:"
                                     custom-class="is-small"
                                     v-if="editing"
                            >
                                <b-input v-model="rasa" custom-class="is-small" expanded></b-input>
                            </b-field>
                            <span v-else>
                                <br/>
                                Rasa: {{ rasa }}
                            </span>
                        </div>
                        <div class="column">
                            <b-field
                                label="Pochodzenie:"
                                custom-class="is-small"
                                v-if="editing"
                            >
                                <b-input v-model="pochodzenie" custom-class="is-small" expanded></b-input>
                            </b-field>
                            <span v-else>
                                Pochodzenie: {{ pochodzenie }}
                            </span>
                        </div>
                    </div>
                </div>
                <div class="is-offset-3 input-box">
                    <div class="columns">
                        <div class="column">
                            <b-field horizontal
                                     label="Profesja:"
                                     custom-class="is-small"
                                     v-if="editing"
                            >
                                <b-input v-model="profesja" custom-class="is-small" expanded></b-input>
                            </b-field>
                            <span v-else>
                                Profesja: {{ profesja }}
                            </span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="column input-box">
                <div class="columns">
                    <div class="column is-one-fifth">
                        <br/><br/>
                        Szybkość
                    </div>
                    <div class="column">
                        <br/>
                        <dice-background style="padding-top: 25%; height: 80px"
                                         v-model="szybkosc">
                        </dice-background>
                    </div>
                    <div class="column is-one-fifth">
                        <br/><br/>
                        Inicjatywa
                    </div>
                    <div class="column">
                        <br/>
                        <dice-background style="padding-top: 25%; height: 80px"
                                         v-model="inicjatywa">
                        </dice-background>
                    </div>
                </div>
            </div>
        </div>
        <div class="columns">
            <div class="column">
                <div class="columns">
                    <div class="column is-one-third">
                        <ul class="has-text-left">
                            <li v-for="(value, st) in stats" class="stats">
                                <div class="columns">
                                    <span class="column has-text-weight-bold is-offset-2 is-one-fifth">
                                        <br/>
                                        {{ st }}
                                    </span>
                                    <div class="column">
                                        <dice-background style="padding-top: 22%; height: 50px"
                                                         v-model="stats[st]">
                                        </dice-background>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                    <div class="column">
                        <div class="columns stats-row">
                            <h1 class="column stat-box has-text-weight-bold is-offset-1">Umiejętności podstawowe</h1>
                        </div>
                        <ul>
                            <li v-for="(value, um) in umPodst" class="columns stats-row">
                                <span class="column stat-box is-four-fifths is-offset-1">
                                    {{ um }}
                                </span>
                                <div class="column">
                                    <dice-background v-model="umPodst[um]" :defaultValue="1">
                                    </dice-background>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
                <br/>
                <div class="columns">
                    <div class="column text-box">
                        <h1 class="input-box has-text-weight-bold has-text-centered">Zdolności personalne</h1>
                        <b-field v-if="editing">
                            <b-input maxlength="500" rows="12" type="textarea" v-model="zdolnosciPersonalne"></b-input>
                        </b-field>
                        <span v-else>{{ zdolnosciPersonalne }}</span>
                    </div>
                </div>
            </div>
            <div class="column is-two-fifths">
                <div class="columns stats-row">
                    <h1 class="column stat-box has-text-weight-bold is-offset-1">Umiejętności zaawansowanie</h1>
                </div>
                <ul>
                    <li v-for="(um, index) in umZaw" class="columns stats-row">
                        <span class="column stat-box is-four-fifths is-offset-1">
                            <b-input v-model="umZaw[index].key" custom-class="is-small" expanded
                                     v-if="editing"></b-input>
                            <span v-else>{{umZaw[index].key}}</span>
                        </span>
                        <div class="column">
                            <dice-background v-model="umZaw[index].value">
                            </dice-background>
                        </div>

                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    import DiceBackground from './DiceBackground.vue'

    export default {
        name: 'FrontPage',
        props: {
            editing: {
                type: Boolean,
                required: true
            },
            value: {
                type: Object,
                required: false
            }
        },
        components: {
            DiceBackground
        },
        data() {
            return {
                imie: '',
                rasa: '',
                pochodzenie: '',
                profesja: '',
                szybkosc: 0,
                inicjatywa: null,
                zdolnosciPersonalne: '',
                stats: {
                    S: null,
                    Wt: null,
                    Zr: null,
                    Int: null,
                    Sm: null,
                    Ch: null,
                    MM: null,
                    Um: null
                },
                umPodst: {
                    'Walka Wręcz': 1,
                    'Walka Dystansowa': 1,
                    'Hazard': 1,
                    'Plotkowanie': 1,
                    'Przeszukiwanie': 1,
                    'Przekonywanie': 1,
                    'Skradanie się': 1,
                    'Spostrzegawczość': 1,
                    'Targowanie się': 1,
                    'Ukrywanie się': 1,
                    'Unik': 1,
                    'Język Wspólny': 1,
                    'Wiedza o Świecie': 1,
                    'Zdolności Magiczne': 1,
                    'Pływanie': 1,
                    'Dowodzenie': 1,
                    'Sztuka Przetrwania': 1,
                    'Zastraszanie': 1,
                    'Jeździectwo': 1
                },
                umZaw: []
            }
        },
        created() {
            for (let i = 0; i < 37; i++) {
                this.umZaw[i] = {
                    key: null,
                    value: null
                }
            }
        },
        watch: {
            $data: {
                handler: function(value) {
                    this.$emit('input', value);
                },
                deep: true
            },
            value(val) {
                for (let property in val) {
                    if (this.$data.hasOwnProperty(property)) {
                        if (isNaN(parseFloat(val[property]))) {
                            this.$data[property] = val[property];
                        } else {
                            this.$data[property] = parseFloat(val[property]);
                        }
                    }
                }
            }
        },
        mounted() {
            this.$emit('input', this.$data);
        }
    }
</script>

<style lang="scss">


</style>
