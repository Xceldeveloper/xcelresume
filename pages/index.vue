<template>
  <vue-html2pdf
    :show-layout="true"
    :float-layout="true"
    :enable-download="true"
    :preview-modal="true"
    :paginate-elements-by-height="1400"
    filename="hee hee"
    :pdf-quality="2"
    :manual-pagination="false"
    pdf-format="a4"
    pdf-orientation="landscape"
    pdf-content-width="800px"
    :html-to-pdf-options="htmlToPdfOptions"
    @beforeDownload="beforeDownload($event)"
    @hasDownloaded="hasDownloaded($event)"
    ref="html2Pdf"
  >
    <section slot="pdf-content">
      xceldeveloper

      <span style="color: red">isaread</span>

      <br />
      <v-btn color="#DC143C">button</v-btn>

      <a href="https://dhhdhdhdh.com">link</a>
      <img :src="imageurl" />
    </section>
  </vue-html2pdf>
</template>

<script>
export default {
  data() {
    return {
      imageurl: require("static/elon.jpg"),
      //https://upload.wikimedia.org/wikipedia/commons/8/85/Elon_Musk_Royal_Society_%28crop1%29.jpg

      htmlToPdfOptions: {
        margin: 0,

        filename: `hehehe.pdf`,

        image: {
          type: "jpeg",
          quality: 0.98,
        },

        enableLinks: true,

        html2canvas: {
          scale: 1,
          useCORS: true,
        },

        jsPDF: {
          unit: "in",
          format: "a4",
          orientation: "portrait",
        },
      },
    };
  },
  mounted() {
    setTimeout(() => {
      this.$refs.html2Pdf.generatePdf();
    }, 3000);
  },

  methods: {
    hasDownloaded() {
      console.log("downloaded");
    },

    async beforeDownload({ html2pdf, options, pdfContent }) {
      await html2pdf()
        .set(options)
        .from(pdfContent)
        .toPdf()
        .get("pdf")
        .then((pdf) => {
          const totalPages = pdf.internal.getNumberOfPages();
          for (let i = 1; i <= totalPages; i++) {
            pdf.setPage(i);
            pdf.setFontSize(10);
            pdf.setTextColor("#000000");
            pdf.text(
              "xceldeveloper.com",
              pdf.internal.pageSize.getWidth() * 0.88,
              pdf.internal.pageSize.getHeight() - 0.3
            );
          }
        })
        .save();
    },

    generateReport() {
      this.$refs.html2Pdf.generatePdf();
    },
  },
};
</script>

<style>
</style>