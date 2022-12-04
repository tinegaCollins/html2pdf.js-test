<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue';
import html2pdf from 'html2pdf.js';
import { ref } from 'vue';  
let options = {
  margin:       1,
  filename:     'myfile.pdf',
  image:        { type: 'jpeg', quality: 0.98 },
  html2canvas:  { scale: 2 },
  jsPDF:        { unit: 'in', format: 'letter', orientation: 'portrait' }
};

//right a function to generate pdf and download it
async function generatePdf(){
  let element = document.getElementById('template');
  console.log(element);
  await html2pdf()
    .from(element)
        // .from(pdfContent)
        .toPdf()
        .get("pdf")
        .then(pdf => {
          const totalPages = pdf.internal.getNumberOfPages();
          console.log(totalPages);
          for (let i = 1; i <= totalPages; i++) {
            pdf.setPage(i);
            pdf.setFontSize(10);
            pdf.setTextColor(150);
            // pdf.addSvgAsImage(
            //   '<?xml version="1.0" encoding="utf-8"?> <!-- Generator: Adobe Illustrator 25.2.1, SVG Export Plug-In . SVG Version: 6.00 Build 0)  --> <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xli',
            //   2,
            //   1.8,
            //   0.10,
            //   0.9
            // );
            pdf.text(
              "Page " + i + " of " + totalPages,
              pdf.internal.pageSize.getWidth() * 0.88,
              pdf.internal.pageSize.getHeight() - 0.3
            );
          }
        })
        .save()
      }

async function getTemplate() {
      const element = document.getElementById("template");
      console.log(element);
      let pdf = await html2pdf()
        .from(element)
        .set(options)
        .toPdf()
        .get("pdf")
        .then(pdf => {
          console.log(pdf.output("datauristring"));
          return pdf.output("datauristring")
        });
      return pdf;
    }
</script>

<template>
  <div>
    <button @click="generatePdf">Download</button>
    <div id="template">
      <h1> Last Will and Testament </h1>
      <div 
      @beforeDownload="beforeDownload($event)" 
      id="template">
        <p>efrwegnmrlogr do hereby make, publish and declare this to be my last will and testament. </p>
      <p> I revoke all former wills and codicils by me at any time made. </p>
      <p> I direct that my body be buried in a decent and Christian manner at the discretion of my executor hereinafter named, and as to my worldly estate, both real and personal, I give, devise and bequeath the same as follows: </p>
      <p> I give, devise and bequeath to my efrwegnmrlogr, all of my real estate, both personal and real, to have and to hold the same to him and his heirs forever. </p>
      <p> I give, devise and bequeath to my daughter, efrwegnmrlogr, all of my personal property, both real and personal, to have and to hold the same to her and her heirs forever. </p>
      <p> I hereby nominate, constitute and appoint my efrwegnmrlogr, to be the executor of this my last will and testament. </p>
      <p> In witness whereof, I have hereunto set my hand and seal this . </p>
      <p> tinega </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
