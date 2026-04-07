<template>
  <div class="attr-item-box">
    <div class="bg-item">
      <Button @click="replaceLeftBoxImage" type="primary" long>左盒面图片</Button>
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

const replaceLeftBoxImage = async () => {
  try {
    const [file] = await selectFiles({ accept: 'image/*', multiple: false });
    if (!file) return;

    const fileStr = await getImgStr(file);
    const imgEl = await insertImgFile(fileStr);

    // 创建图片对象时设置所有属性
    const image = new fabric.Image(imgEl, {
      left: 359,
      top: 668,
      width: 540,
      height: 750,
      angle: 0,
      originX: 'left',
      originY: 'top',
    });

    // 添加到画布
    canvasEditor.canvas.add(image);
    // 设置3d旋转效果
    image.set('angleY', 50);
    const skewY = Math.sin((50 * Math.PI) / 180) * 30;
    image.set('skewY', skewY);
    // 设置为活动对象
    canvasEditor.canvas.setActiveObject(image);
    // 重新渲染画布
    canvasEditor.canvas.renderAll();

    imgEl.remove();
  } catch (error) {
    // ignore error
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
