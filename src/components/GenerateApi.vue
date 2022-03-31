<template>
  <button class="btb btn-primary m-5" @click="loadData">Load Data</button>
  <button class="btb btn-success m-5" @click="createPost">Create Post</button>
  <button class="btb btn-info m-5" @click="updatePost">Update Post</button>
  <button
    v-if="apiData.length != 0"
    class="btb btn-danger m-5"
    @click="excelFunc"
  >
    Generate Excel From Api
  </button>

  <button
    v-if="apiData.length != 0"
    @click="generateTable"
    class="btb btn-success m-5"
  >
    Generate Pdf From Api
  </button>
  <div class="container card" style="width: 18rem">
    <p>Posts Titles</p>
    <ul v-if="apiData.length != 0" class="list-group list-group-flush">
      <li
        @click="loadPostComment(post.id)"
        v-for="post in apiData"
        :key="post.title"
        class="list-group-item"
      >
        {{ post.title }}
      </li>
    </ul>
    <hr />
    <p v-if="comments.length != 0">Post/Comments</p>
    <ul v-if="comments.length != 0" class="list-group list-group-flush">
      <li
        v-for="comment in comments"
        :key="comment.name"
        class="list-group-item"
      >
        {{ comment.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import { jsPDF } from "jspdf";
import "jspdf-autotable";
let xlsx = require("json-as-xlsx");

export default {
  data() {
    return {
      apiData: [],
      items: [
        { title: "Item 1", body: "I am item 1 body text" },
        { title: "Item 2", body: "I am item 2 body text" },
        { title: "Item 3", body: "I am item 3 body text" },
        { title: "Item 4", body: "I am item 4 body text" },
      ],
      body: [],
      comments: [],
    };
  },
  methods: {
    loadData() {
      this.apiData = fetch(
        "https://jsonplaceholder.typicode.com/posts?_limit=5"
      )
        .then((response) => response.json())
        .then((json) => (this.apiData = json));
    },
    loadPostComment(id) {
      this.apiData = fetch(
        `https://jsonplaceholder.typicode.com/posts/${id}/comments`
      )
        .then((response) => response.json())
        .then((json) => (this.comments = json));
    },
    generateApiPdf() {
      const doc = new jsPDF({
        orientation: "portrait",
        unit: "in",
        format: "letter",
      });
      doc.setFontSize(16).text("Posts Titles", 1, 0.5);
      doc.setLineWidth(0.01).line(0.5, 1.1, 8.0, 1.1);

      let y_axis = 1;
      this.apiData.map((item) => {
        doc
          .setFont("helvetica")
          .setFontSize(12)
          .text(item.title, 1, y_axis, { align: "left", maxWidth: "7.5" });

        y_axis = y_axis + 0.5;
      });
      doc.save("api.pdf");
    },
    generateTable() {
      var doc = new jsPDF();
      var col = ["Title", "Body"];
      var rows = [];

      /* The following array of object as response from the API req  */

      this.apiData.forEach((element) => {
        var temp = [element.title, element.body];
        rows.push(temp);
        rows.push(temp);
      });

      doc.autoTable(col, rows, { startY: 10 });

      doc.save("Test.pdf");
    },
    createPost() {
      // data to be sent to the POST request
      let _data = {
        title: "my new post title",
        body: "body",
        userId: 1,
      };

      fetch("https://jsonplaceholder.typicode.com/posts", {
        method: "POST",
        body: JSON.stringify(_data),
        headers: { "Content-type": "application/json; charset=UTF-8" },
      })
        .then((response) => response.json())
        .then((json) => alert("new post added" + json.title))
        .catch((err) => console.log(err));
    },
    updatePost() {
      // data to be sent to the POST request
      let _data = {
        title: "my new post title",
        body: "body",
      };

      fetch("https://jsonplaceholder.typicode.com/posts/1", {
        method: "PUT",
        body: JSON.stringify(_data),
        headers: { "Content-type": "application/json; charset=UTF-8" },
      })
        .then((response) => response.json())
        .then((json) => alert(json.title + " post has updated"));
    },
    excelFunc() {
      let arr = [];
      this.apiData.forEach((item) => {
        arr.push(item);
      });
      let data = [
        {
          sheet: "Posts",
          columns: [
            { label: "title", value: "title" }, // Top level data
            { label: "body", value: "body" }, // Column format
            {
              label: "Title",
              value: "user.more.phone",
              format: "(###) ###-####",
            }, // Deep props and column format
          ],
          content: arr,
        },
      ];

      let settings = {
        fileName: "MySpreadsheet", // Name of the resulting spreadsheet
        extraLength: 3, // A bigger number means that columns will be wider
        writeOptions: {}, // Style options from https://github.com/SheetJS/sheetjs#writing-options
      };

      xlsx(data, settings); // Will download the excel file
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
