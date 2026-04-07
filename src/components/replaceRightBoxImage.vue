<template>
  <div class="attr-item-box">
    <div class="bg-item">
      <Button @click="replaceRightBoxImage" type="primary" long>右盒面图片</Button>
    </div>
  </div>
</template>

<script setup>
import { Button } from 'view-ui-plus';
import useSelect from '@/hooks/select';
import { Utils } from '@kuaitu/core';
import { fabric } from 'fabric';

const { getImgStr, selectFiles, insertImgFile } = Utils;
const { canvasEditor } = useSelect();

const replaceRightBoxImage = async () => {
  try {
    const [file] = await selectFiles({ accept: 'image/*', multiple: false });
    if (!file) return;

    const fileStr = await getImgStr(file);
    const imgEl = await insertImgFile(fileStr);

    // 创建图片对象时设置所有属性
    const image = new fabric.Image(imgEl, {
      left: 896,
      top: 654.76,
      width: 540,
      height: 775,
      angle: 0,
      originX: 'left',
      originY: 'top',
    });

    // 添加到画布
    canvasEditor.canvas.add(image);
    // 设置3d旋转效果
    image.set('angleY', 313);
    const skewY = Math.sin((313 * Math.PI) / 180) * 30;
    image.set('skewY', skewY);
    // 设置为活动对象
    canvasEditor.canvas.setActiveObject(image);
    // 重新渲染画布
    canvasEditor.canvas.renderAll();

    imgEl.remove();
  } catch (error) {
    console.error('替换右盒面图片失败:', error);
  }
};
</script>

<style scoped lang="less">
.attr-item-box {
  margin-top: 8px;
}

.bg-item {
  text-align: center;
}
</style>
