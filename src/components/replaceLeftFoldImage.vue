<template>
  <div class="attr-item-box">
    <div class="bg-item">
      <Button @click="replaceLeftFoldImage" type="primary" long>替换左折页图片</Button>
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

const replaceLeftFoldImage = async () => {
  try {
    const [file] = await selectFiles({ accept: 'image/*', multiple: false });
    if (!file) return;

    const fileStr = await getImgStr(file);
    const imgEl = await insertImgFile(fileStr);

    // 创建图片对象时设置所有属性
    const image = new fabric.Image(imgEl, {
      left: 437.6,
      top: 534.88,
      width: 574,
      height: 293,
      angle: 34,
      originX: 'left',
      originY: 'top',
    });

    // 添加到画布
    canvasEditor.canvas.add(image);
    // 设置3d旋转效果
    image.set('angleY', 210);
    const skewY = Math.sin((210 * Math.PI) / 180) * 30;
    image.set('skewY', skewY);
    // 设置为活动对象
    canvasEditor.canvas.setActiveObject(image);
    // 重新渲染画布
    canvasEditor.canvas.renderAll();

    imgEl.remove();
  } catch (error) {
    console.error('替换左折页图片失败:', error);
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
