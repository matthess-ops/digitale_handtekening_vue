<template>
  <div>
    <h1>documents overview</h1>
    <!-- userID {{ userId }}
    <div>
      <h4>documents:</h4>
      <div>{{ documents }}</div>
    </div> -->

    <button @click="downloadtxt()">DownLoad txt</button>
    <button @click="downloadpdf()">DownLoad pdf</button>

    <!-- <div></div> -->
    <!-- <button v-on:click="huh()">Add 1</button> -->

    <!-- <button @click="downloadFile()">doehet</button> -->

    <div v-if="loading">Data is loading...</div>
    <div v-else>
      <div v-for="document in documents" :key="document.id">
        <span>{{ document.id }}</span>
        <span>{{ document.filename }}</span>
        <button @click="download(document.id)">download</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      documents: null,
      loading: true,
      userId: this.$store.state.auth.user.id,
    };
  },
  computed: {},
  methods: {
    huh() {
      console.log("kom op");
    },
    getDocuments() {
      this.loading = true;

      axios.get(`/api/documents`).then((response) => {
        if (response.status == 200) {
          this.documents = response.data;
        } else {
          console.log("Error occurred " + response.data.status);
        }
        this.loading = false;
      });
    },
    download(id) {
      axios.get(`/api/download/${id}`).then((response) => {
        if (response.status == 200) {
          console.log("download worked");
          // this.documents = response.data;
        } else {
          console.log("Error occurred " + response.data.status);
        }
        this.loading = false;
      });
    },

    testresponse() {
      axios.get(`/api/testresponse`).then((response) => {
        console.log(response);
      });
    },

    downloadFile() {
      console.log("wat");
      axios
        .get("/api/downloaddddffdd", { responseType: "blob" })

        .then((res) => {
          console.log(res);
          var FILE = window.URL.createObjectURL(new Blob([res.data]));

          var docUrl = document.createElement("x");
          docUrl.href = FILE;
          docUrl.setAttribute("download", "file.pdf");
          document.body.appendChild(docUrl);
          docUrl.click();
          docUrl.remove();
        });
    },

    downloadpdf() {
      axios
        .get(`/api/downloadpdf/${3}`, {responseType: 'arraybuffer'})
        .then((response) => {
          console.log(response.data);
          let blob = new Blob([response.data], { type: "application/pdf" });
          let link = document.createElement("a");
          link.href = window.URL.createObjectURL(blob);
          link.download = "testt.pdf";
          link.click();

          console.log("donwloaded pdf");
        });
    },

    downloadtxt() {
      //https://stackoverflow.com/questions/67983200/download-file-from-backend-laravel-using-vue-as-frontend
      axios
        .post("/api/downloadtxt", { responseType: "blob" })
        .then((response) => {
          try {
            // Service that handles ajax call
            // console.log(response.data.name);
            const url = window.URL.createObjectURL(new Blob([response.data]));
            const link = document.createElement("a");
            link.href = url;
            link.setAttribute("download", "testggrtt.txt");
            document.body.appendChild(link);
            link.click();
            // link.remove();

            console.log("donwloaded txt");
          } catch (error) {
            console.error(error);
          }
        });
    },
  },

  beforeMount() {
    this.getDocuments();
  },
  mounted() {},

  created() {},
};
</script>
