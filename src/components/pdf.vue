<template>
  <div id="pdf">
    <div class="pdfbuttom">
      <!-- 是否显示 -->
      <input type="checkbox" v-model="show">Show/Hidden
      <!-- 按钮组件 -->
      <el-button-group>
        <el-button type="primary" icon="el-icon-arrow-left" @click="red">Previous page</el-button>
        <el-button type="primary" @click="add">Next page<i class="el-icon-arrow-right el-icon--right"></i></el-button>
      </el-button-group>
      <!-- 显示页码 -->
      <b style="font-size: 18px"> {{page}} /{{numPages}}</b>
      <el-button @click="rotate += 90">&#x27F3;</el-button>
      <el-button @click="rotate -= 90">&#x27F2;</el-button>
      <el-button type="info" @click="$refs.pdf.print()">Print</el-button>
      <el-button type="info" @click="savePdf()">Save</el-button>
    </div>
    <!-- 这里是显示pdf的地方 -->
    <div class="showpdf">
      <!--<pdf v-if="show" ref="pdf" :src="src" class="pdfbook" :page="page" :rotate="rotate" @password="password"-->
      <!--@progress="loadedRatio = $event" @error="error" @num-pages="numPages = $event"></pdf>-->

    </div>
  </div>
</template>
<script>
  import pdf from 'vue-pdf'

  export default {
    props: [
      'v_show', 'v_src',
      'v_loadedRatio',
      'v_page',
      'v_numPages',
      'v_rotate'],

    components: {
      pdf: pdf
    },
    data() {
      return {
        show: true,
        src: 'http://localhost:8090/static/docker.pdf',
        loadedRatio: 0,
        page: 1,
        numPages: 0,
        rotate: 0,
      }
    },
    beforeMount() {
      for (let i = 0; i < this.numPages; i++) {
        var pdfText = "<pdf v-if=\"show\" ref=\"pdf\" :src=\"src\" class=\"pdfbook\" :page=\"\" :rotate=\"rotate\" @password=\"password\"\n" +
          "           @progress=\"loadedRatio = $event\" @error=\"error\" @num-pages=\"numPages = $event\"></pdf>";
        pdfText.append(pdfText);
      }
      document.getElementsByClassName('showpdf').innerHTML = pdfText;
    },
    methods: {
      password: function (updatePassword, reason) {
        updatePassword(prompt('password is "test"'));
      },
      error: function (err) {
        console.log(err);
      },
      red() {
        if (this.page > 1) {
          this.page = this.page - 1;
        }
      },
      add() {
        if (this.page < this.numPages) {
          this.page = this.page + 1;
        }
      },
      savePdf() {
        // window.open('http://localhost:8090/static/Piano.pdf','targetWindow','toolbar=no,location=no,status=no,menubar=no,scrollbars=yes,resizable=yes,width=750,height=550');
      }
    }
  }
</script>

<style>
  #pdf {
    width: 100%;
  }

  .pdfbook {
    border: 2px solid black
  }

  .showpdf {
    width: 100%;
    margin: 0 auto;
  }

  .pdfbuttom {
    left: 0;
    position: fixed;
    top: 0;
    z-index: 9999;
    width: 100%;
    height: 42px;
    background-color: #00a087;
  }

</style>
