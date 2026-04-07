<!--
 * @Author: 秦少卫
 * @Date: 2022-09-03 19:16:55
 * @LastEditors: June
 * @LastEditTime: 2024-11-22 15:28:43
 * @Description: 尺寸设置
-->

<template>
  <div class="attr-item-box">
    <div v-if="!isSelect">
      <!-- 画布尺寸设置 -->
      <Divider plain orientation="left">
        <h4>{{ $t('bgSeting.size') }}</h4>
      </Divider>
      <Form :label-width="40" inline class="form-wrap">
        <FormItem :label="$t('bgSeting.width')" prop="name">
          <InputNumber disabled v-model="width" readonly @on-change="setSize"></InputNumber>
        </FormItem>
        <FormItem :label="$t('bgSeting.height')" prop="name">
          <InputNumber disabled v-model="height" readonly @on-change="setSize"></InputNumber>
        </FormItem>
        <FormItem :label-width="0">
          <Button type="text" @click="showSetSize">
            <Icon type="md-create" />
          </Button>
        </FormItem>
      </Form>

      <!-- 修改尺寸 -->
      <modalSzie :title="$t('setSizeTip')" ref="modalSizeRef" @set="handleConfirm"></modalSzie>
    </div>

    <!-- 替换图片功能 -->
    <Divider plain orientation="left">
      <h4>插入盒子图片</h4>
    </Divider>
    <div class="replace-image-group">
      <Button @click="replaceLeftFoldImage" type="primary" long>左折页图片</Button>
      <Button @click="replaceRightFoldImage" type="primary" long>右折页图片</Button>
      <Button @click="replaceLeftBoxImage" type="primary" long>左盒面图片</Button>
      <Button @click="replaceRightBoxImage" type="primary" long>右盒面图片</Button>
    </div>
  </div>
</template>

<script setup name="CanvasSize">
import { ref, onMounted } from 'vue';
import { Button, Divider, Form, FormItem, InputNumber, Icon } from 'view-ui-plus';
import useSelect from '@/hooks/select';
import modalSzie from '@/components/common/modalSzie';
import { Utils } from '@kuaitu/core';
import { fabric } from 'fabric';

const { getImgStr, selectFiles, insertImgFile } = Utils;
const { isSelect, canvasEditor } = useSelect();

const modalSizeRef = ref(null);

const width = ref(0);
const height = ref(0);

onMounted(() => {
  const size = canvasEditor.getWorkspase();
  const { width: w, height: h } = size || {};
  width.value = w;
  height.value = h;
  canvasEditor.on('sizeChange', (w, h) => {
    width.value = w;
    height.value = h;
  });
});

const setSize = () => {
  canvasEditor.setSize(width.value, height.value);
};

const showSetSize = () => {
  modalSizeRef.value.showSetSize(width.value, height.value);
};
const handleConfirm = (w, h) => {
  width.value = w;
  height.value = h;
  setSize();
};

// 替换左折页图片
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
    alert('替换左折页图片失败，请重试');
  }
};

// 替换右折页图片
const replaceRightFoldImage = async () => {
  try {
    const [file] = await selectFiles({ accept: 'image/*', multiple: false });
    if (!file) return;

    const fileStr = await getImgStr(file);
    const imgEl = await insertImgFile(fileStr);

    // 创建图片对象时设置所有属性
    const image = new fabric.Image(imgEl, {
      left: 1589.41,
      top: 908.4,
      width: 574,
      height: 290,
      angle: 150,
      originX: 'left',
      originY: 'top',
    });

    // 添加到画布
    canvasEditor.canvas.add(image);
    // 设置3d旋转效果
    image.set('angleY', 20);
    const skewY = Math.sin((20 * Math.PI) / 180) * 30;
    image.set('skewY', skewY);
    // 设置为活动对象
    canvasEditor.canvas.setActiveObject(image);
    // 重新渲染画布
    canvasEditor.canvas.renderAll();

    imgEl.remove();
  } catch (error) {
    console.error('替换右折页图片失败:', error);
  }
};

// 替换左盒面图片
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
    console.error('替换左盒面图片失败:', error);
  }
};

// 替换右盒面图片
const replaceRightBoxImage = async () => {
  try {
    const [file] = await selectFiles({ accept: 'image/*', multiple: false });
    if (!file) return;

    const fileStr = await getImgStr(file);
    const imgEl = await insertImgFile(fileStr);

    // 创建图片对象时设置所有属性
    const image = new fabric.Image(imgEl, {
      left: 908.44,
      top: 662,
      width: 540,
      height: 750,
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
:deep(.ivu-form-item) {
  margin-bottom: 0;
}

:deep(.ivu-input-number) {
  width: 70px;
}

.form-wrap {
  display: flex;
}

.replace-image-group {
  margin-top: 16px;
  display: flex;
  flex-direction: column;
  gap: 8px;

  :deep(.ivu-btn) {
    margin-bottom: 8px;
  }
}
</style>
