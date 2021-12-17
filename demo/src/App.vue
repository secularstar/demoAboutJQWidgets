<template>
  <div>
    <!-- 第一个: jqxTab的渲染问题 -->

    <div>1. 需要将数据集的3个 title , 渲染到jqxTab上面</div>
    <br />
    <JqxTabs
      ref="myTabs"
      :height="200"
      :position="'top'"
      :animationType="'none'"
      :selectionTracker="false"
    >
      <ul>
        <li
          v-for="(jqxTabItem, jqxTabIndex) in jqxTabList.title"
          :key="jqxTabIndex"
          style="margin-left: 30px"
        >
          {{ jqxTabItem }}
        </li>
      </ul>
      <div></div>
    </JqxTabs>

    <br />
    <div>
      2. jqxGrid中使用 jqxDropDownList的时候, 单选和多选,
      需要选择的时候,展示的是label, 而传值给后台的时候, 给的是value
    </div>
    <JqxGrid
      ref="myGrid"
      :width="800"
      @cellvaluechanged="onCellvaluechanged($event,index)"
      :theme="'material-green'"
      :source="dataAdapter"
      :columns="columns"
      :editable="true"
    />

    <br />

    <div>
      3. jqxForm的key需要动态渲染出来 , 点击按钮,
      将newTemplate的数据替换显示到form上
    </div>
    <JqxButton
      ref="button"
      :width="120"
      :height="40"
      :textPosition="'center'"
      v-on:click="jqxFormnButtonClicked()"
    >
      jqxFormnButton
    </JqxButton>
    <JqxForm
      ref="myForm"
      style="width: 400px; height: auto"
      :template="template"
      :value="value"
    >
    </JqxForm>
  </div>
</template>

<script>
import JqxGrid from "jqwidgets-scripts/jqwidgets-vue/vue_jqxgrid.vue";
import JqxTabs from "jqwidgets-scripts/jqwidgets-vue/vue_jqxtabs.vue";
import JqxForm from "jqwidgets-scripts/jqwidgets-vue/vue_jqxform.vue";
import JqxButton from 'jqwidgets-scripts/jqwidgets-vue/vue_jqxbuttons.vue';

export default {
  components: {
    JqxGrid,
    JqxTabs,
    JqxForm,
    JqxButton
  },
  data: function () {
    return {
      // 1. jqxTab需要的数据
      jqxTabList: {
        title: ["数据集1", "数据集2", "数据集3"],
        content: ["数据集1内容", "数据集2内容", "数据集3内容"],
      },
      // 2. jqxDropDownList需要的数据
      dataAdapter: new jqx.dataAdapter(this.source),
      dictionaryOptionsSource: {
        datatype: "json",
        datafields:
            [
              {name: 'label', type: 'string'},
              {name: 'value', type: 'string'}
            ],
        localdata: this.countryOptions
      },
      columns: [
        { text: "Contact Name", datafield: "ContactName", width: 240 },
        { text: "Contact Title", datafield: "Title", width: 240 },
        {
          text: "City",
          datafield: "City",
          columntype:"dropdownlist",
          createeditor: async (
            row,
            cellvalue,
            editor,
            cellText,
            width,
            height
          ) => {
            this.dictionaryOptionsSource.localdata = this.countryOptions;
            let dAdapter = new jqx.dataAdapter(this.dictionaryOptionsSource);
            editor.jqxDropDownList({
              checkboxes: false,
              source: dAdapter,
              displayMember: "label",
              valueMember: "value",
              width: width,
              height: height,
              selectedIndex: 0,
            });
          }
        },
        {
          text: "Country",
          datafield: "Country",
          columntype:"dropdownlist",
          createeditor: async (
            row,
            cellvalue,
            editor,
            cellText,
            width,
            height
          ) => {
            this.dictionaryOptionsSource.localdata = this.countryOptions;
            let dAdapter = new jqx.dataAdapter(this.dictionaryOptionsSource);
            console.log(this.dictionaryOptionsSource)
            editor.jqxDropDownList({
              checkboxes: true,
              source: dAdapter,
              displayMember: "label",
              valueMember: "value",
              width: width,
              height: height,
            });
          }
        },
      ],
      countryOptions: [
        { label: "中国", value: "zh" },
        { label: "德国", value: "de" },
        { label: "美国", value: "us" },
      ],
      // jqxForm需要的数据
      template: [
        {
          bind: "firstName",
          type: "text",
          label: "First name",
          required: true,
          labelWidth: "85px",
          width: "250px",
          info: "Enter first name",
          infoPosition: "right",
        },
        {
          bind: "lastName",
          type: "text",
          label: "Last name",
          required: true,
          labelWidth: "85px",
          width: "250px",
          info: "Enter last name",
          infoPosition: "right",
        },
        {
          bind: "company",
          type: "text",
          label: "Company",
          required: false,
          labelWidth: "85px",
          width: "250px",
        },
        {
          bind: "address",
          type: "text",
          label: "Address",
          required: true,
          labelWidth: "85px",
          width: "250px",
        },
        {
          bind: "city",
          type: "text",
          label: "City",
          required: true,
          labelWidth: "85px",
          width: "250px",
        },
        {
          bind: "state",
          type: "option",
          label: "State",
          required: true,
          labelWidth: "85px",
          width: "250px",
          component: "jqxDropDownList",
          options: [
            { value: "California" },
            { value: "New York" },
            { value: "Oregon" },
            { value: "Illinois" },
            { value: "Texas" },
          ],
        },
        {
          bind: "zip",
          type: "text",
          label: "Zip code",
          required: true,
          labelWidth: "85px",
          width: "250px",
        },
        {
          type: "blank",
          rowHeight: "10px",
        },
      ],
      newTemplate: [
        {
          bind: "schoolName",
          type: "text",
          label: "School name",
          required: true,
          labelWidth: "85px",
          width: "250px",
          info: "Enter school name",
          infoPosition: "right",
        },
        {
          bind: "className",
          type: "text",
          label: "Class name",
          required: true,
          labelWidth: "85px",
          width: "250px",
          info: "Enter class name",
          infoPosition: "right",
        },
        {
          bind: "studentName",
          type: "text",
          label: "Student name",
          required: true,
          labelWidth: "85px",
          width: "250px",
          info: "Enter student name",
          infoPosition: "right",
        },
      ],
      value: {
        firstName: "John",
        lastName: "Scott",
        address: "1st Ave SW",
        city: "San Antonio",
        state: "Texas",
        zip: "78209",
      },
      indexValue: 0,
    };
  },
  beforeCreate: function () {
    this.source = {
      localdata: [
        ["Maria Anders", "Sales Representative", "Berlin", "Germany"],
        ["Ana Trujillo", "Owner", "Mxico D.F.", "Mexico"],
        ["Antonio Moreno", "Owner", "Mxico D.F.", "Mexico"],
        ["Thomas Hardy", "Sales Representative", "London", "UK"],
        ["Christina Berglund", "Order Administrator", "Lule", "Sweden"],
        ["Hanna Moos", "Sales Representative", "Mannheim", "Germany"],
        ["Frdrique Citeaux", "Marketing Manager", "Strasbourg", "France"],
        ["Martn Sommer", "Owner", "Madrid", "Spain"],
        ["Owner", "Marseille", "France"],
        ["Elizabeth Lincoln", "Accounting Manager", "Tsawassen", "Canada"],
        ["Victoria Ashworth", "Sales Representative", "London", "UK"],
        ["Patricio Simpson", "Sales Agent", "Buenos Aires", "Argentina"],
        ["Francisco Chang", "Marketing Manager", "Mxico D.F.", "Mexico"],
        ["Yang Wang", "Owner", "Bern", "Switzerland"],
        ["Pedro Afonso", "Sales Associate", "Sao Paulo", "Brazil"],
        ["Elizabeth Brown", "Sales Representative", "London", "UK"],
        ["Sven Ottlieb", "Order Administrator", "Aachen", "Germany"],
        ["Janine Labrune", "Owner", "Nantes", "France"],
        ["Ann Devon", "Sales Agent", "London", "UK"],
        ["Roland Mendel", "Sales Manager", "Graz", "Austria"],
      ],
      datafields: [
        { name: "ContactName", type: "string", map: "0" },
        { name: "Title", type: "string", map: "1" },
        { name: "City", type: "string", map: "2" },
        { name: "Country", type: "string", map: "3" },
      ],
      datatype: "array",
    };
  },
  methods: {
    // 1. jqxTabs 中的方法;

    // 2. jqxDropDownList 中的方法;
    onCellvaluechanged(event, index) {
      console.log(this.$refs.myGrid.getrowdata(event.args.rowindex));
    },

    // 3. jqxForm 中的方法;
    jqxFormnButtonClicked() {
      // 获取新的keys
      let keys;
      switch (this.indexValue) {
        case 0:
          keys = this.newTemplate;
          this.indexValue = 1;
          break;
        case 1:
          keys = this.template;
          this.indexValue = 0;
          break;
        default:
          break;
      }

      console.log("待渲染的keys:", keys);
      // 渲染keys到form
    },
  },
};
</script>Î

<style src='./assets/styles/jqwidgets/jqx.base.css'></style>
<style src='./assets/styles/jqwidgets/jqx.material-green.css'></style>