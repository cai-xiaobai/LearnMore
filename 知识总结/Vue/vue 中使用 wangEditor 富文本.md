vue 中使用 wangEditor 富文本

```vue
<template>
    <div>
        <div ref="editor" style="text-align:left; margin-bottom:20px"></div>
    </div>
</template>

<script>
    import editor from 'wangeditor'

    export default {
      name: 'SimpleEditor',
      model : { prop : 'height' } ,
      props : {
        height : { type : Number , default : 480}
      },
      data () {
        return {
          editor : ""
        }
      },
      mounted() {
        this.$nextTick(function() {
          this.initEditor();
        })
      },
      methods : {
        initEditor() {
          this.editor = new editor(this.$refs.editor)
          this.uploadImg(); // 配置图片
          this.setMenus(); // 配置菜单
          this.editor.create(); // 创建
          this.editor.change = () => {
            this.$emit('changelHtml' , this.editor.txt.html() );
          }
        },
        // 设置菜单
        setMenus () {
          this.editor.customConfig.menus = [
                'undo', // 撤销
                'redo', // 重复
                'head', // 标题
                'bold', // 粗体
                'fontSize', // 字号
                'fontName', // 字体
                'italic', // 斜体
                'strikeThrough', // 删除线
                'foreColor', // 文字颜色
                'backColor', // 背景颜色
                'justify', // 对齐方式
                'image',  // 插入图片
          ]
        },
        // 上传图片
        uploadImg() {
          this.editor.customConfig.showLinkImg = false;
          // 配置服务器端地址
          this.editor.customConfig.uploadImgServer = '/admin/sys/upload';
          //重写上传图片方法
          this.editor.customConfig.customUploadImg = (files, insert) => {
            const form = files.reduce((form , file) => (form.append('files' , file) , form) , new FormData())
            this.http.post('/admin/sys/upload' , form ).then(items => {
              items = items.map(({ url }) => url)
              // 逐个上传
              console.log(items)
              items.map((item) => insert(item))
            })
          };
        },
        getHtml() { 
          return this.editor.txt.html();
        },
        setHtml(txt) {
          this.editor.txt.html(txt);
        },
      }
    }
</script>

<style scoped>
</style>

```

