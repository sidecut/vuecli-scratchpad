<template>
    <div>
        <div>Grab JSON:
            <button @click="getJson" :disabled="fetching">Grab JSON</button>
        </div>
        <div class="pre">{{ results }}</div>
        <!-- <textarea :value="results | json"></textarea> -->
    </div>
</template>

<script>
export default {
    data() {
        return {
            results: "",
            fetching: false
        }
    },
    methods: {
        getJson() {
            let self = this;
            this.results = "";
            this.fetching = true;
            $.getJSON("http://localhost:3000/db")
                .done((data) => {
                    self.results = data;
                    self.fetching = false;
                }).fail((jqxhr, textStatus, error) => {
                    alert(JSON.stringify(error));
                    self.fetching = false;
                });
        }
    }
}
</script>

<style scoped>
.pre {
    font-family: Courier New, Courier, monospace;
    white-space: pre;
}
</style>
