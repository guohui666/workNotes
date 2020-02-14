<template>
    <div class="is">
<!--        html规范-> tbody里只能有tr 否则会渲染错误 -->
        <table>
            <tbody>
                <tr>
                    <td></td>
                </tr>
            </tbody>
        </table>
<!--        使用is指定组件，避免和html规范冲突 其他配对的标签最好也指定，比如ul li -->
        <table>
            <tbody>
                <tr is="row">
                    <td></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        components:{
          row //假设有row子组件
        },
        props: {}
    }
</script>
