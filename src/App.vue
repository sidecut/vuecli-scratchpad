<template>
    <div>
        <p>GET JSON:
            <button @click="getJson" :disabled="fetching">Get JSON</button>
        </p>
        <p>POST some JSON via jQuery:
            <button @click="postJson" :disabled="fetching">Post JSON</button>
            <div>For POST, check devtools network view to see what went over the wire.</div>
        </p>
        <p>POST some JSON via axios:
            <button @click="postJsonAxios" :disabled="fetching">Post JSON</button>
            <div>For POST, check devtools network view to see what went over the wire.</div>
        </p>
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
                }).fail((jqxhr, textStatus, error) => {
                    alert(JSON.stringify(error));
                }).always(() => {
                    self.fetching = false;
                });
        },
        postJson() {
            console.log("POSTing via jQuery", this.results);
            let self = this;
            this.fetching = true;
            try {
                $.post({
                    url: "http://localhost:3000/db",
                    data: JSON.stringify(this.results),
                    dataType: 'json',
                    contentType: 'application/json'
                }).done(() => {
                    self.data = 'Success POSTing.';
                }).fail((xhr, status, error) => {
                    console.log("failed to post", xhr, status, error);
                    self.results = { xhr, status, error };
                }).always(() => {
                    self.fetching = false;
                });
            }
            catch (error) {
                console.log("Error in $.post", error);
                this.results = `Error in $.post: ${error}`;
                self.fetching = false;
            }
        },
        postJsonAxios() {
            console.log("POSTing via axios", this.results);
            let self = this;
            this.fetching = true;
            try {
                axios.post("http://localhost:3000/db", this.results)
                    .then((response) => {
                        console.log("axios POST response", response);
                        self.data = 'Success POSTing.';
                        self.fetching = false;
                    }).catch((error) => {
                        console.log("failed to post", error);
                        self.results = { error };
                        self.fetching = false;
                    });
            }
            catch (error) {
                console.log("Error in $.post", error);
                this.results = `Error in $.post: ${error}`;
                self.fetching = false;
            }
        },
    }
}
</script>

<style scoped>
.pre {
    font-family: Courier New, Courier, monospace;
    white-space: pre;
}
</style>
