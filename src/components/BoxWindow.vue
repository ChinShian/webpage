<template>
  <div class="windows">
    <img
      src="@/assets/images/icon.png"
      alt=""
      class="icon"
      :class="{ active_icon: isActive }"
      @click="toggleBoxWindow"
    />
    <form
      class="window_boxs"
      :class="{ active_windows: isActive }"
      @submit.prevent="handleSubmit"
    >
      <div class="window_box">
        <div class="box_title">
          <h1>意見反饋</h1>
          <button type="button" @click="toggleBoxWindow">X</button>
        </div>
        <div class="box_inputs">
          <div class="input_select">
            <div class="select_arr">
              <select required>
                <option value="" disabled selected hidden>請選擇類別</option>
                <option value="操作問題">操作問題</option>
                <option value="優化建議">優化建議</option>
                <option value="Bug反饋">Bug反饋</option>
                <option value="其他">其他</option>
              </select>
            </div>
            <label for=""><span>*</span>類別</label>
          </div>
          <div class="input_text">
            <input
              type="text"
              placeholder="請輸入標題"
              maxlength="30"
              @input="autoResize"
              required
            />
            <label for=""><span>*</span>標題<small>(限30字符內)</small></label>
          </div>
          <div class="input_textarea">
            <textarea
              ref="textarea"
              placeholder="請描述您的意見/問題"
              maxlength="300"
              @input="autoResize"
              required
            />
            <label for=""><span>*</span>描述<small>(限300字符內)</small></label>
          </div>
          <div class="input_file">
            <h2>參考圖片<small>(僅支持PNG、JPG格式，每張5MB內)</small></h2>
            <div class="input_fileimages">
              <div class="image_previews" v-if="uploadedImages.length > 0">
                <div
                  v-for="(image, index) in uploadedImages"
                  :key="index"
                  class="image_container"
                >
                  <img :src="image" class="image_preview" />
                  <div class="overlay">
                    <button class="overlay_icon enlarge">
                      <img src="@/assets/images/loupe.png" alt="" />
                    </button>
                    <button
                      class="overlay_icon delete"
                      @click="removeImage(index)"
                    >
                      <img src="@/assets/images/bin.png" alt="" />
                    </button>
                  </div>
                </div>
              </div>
              <input
                type="file"
                id="fileimages"
                accept="image/png, image/jpg"
                multiple
                @change="previewImages"
                v-if="uploadedImages.length < 3"
              />
              <label for="fileimages" v-if="uploadedImages.length < 3">
                <span class="fileimages-plus">+</span>
              </label>
            </div>
            <p>
              可提供意見/問題截圖 (上傳數量
              <span>{{ uploadedImages.length }}</span
              >/3)
            </p>
          </div>
          <button type="submit" id="submit" class="submit">提交</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "BoxWindow",
  setup() {
    const isActive = ref(false);
    const uploadedImages = ref([]);
    const textarea = ref(null);

    const toggleBoxWindow = () => {
      isActive.value = !isActive.value;
    };

    const handleSubmit = () => {
      console.log("表單已提交");
    };

    const previewImages = (event) => {
      const files = event.target.files;
      const newImages = [];

      for (let i = 0; i < files.length; i++) {
        const file = files[i];

        if (file.type !== "image/png" && file.type !== "image/jpeg") {
          alert("請上傳 PNG 或 JPG 格式的圖片");
          continue;
        }

        const reader = new FileReader();
        reader.onload = (e) => {
          newImages.push(e.target.result);
          if (newImages.length > 0) {
            uploadedImages.value.push(...newImages);
          }
        };
        reader.readAsDataURL(file);
      }
    };
    const removeImage = (index) => {
      uploadedImages.value.splice(index, 1);
    };

    const autoResize = () => {
      const textareaElement = textarea.value;
      textareaElement.style.height = "41px";
      textareaElement.style.height = `${textareaElement.scrollHeight}px`;
    };

    onMounted(() => {
      autoResize({ target: textarea.value });
    });

    return {
      isActive,
      uploadedImages,
      textarea,
      toggleBoxWindow,
      handleSubmit,
      previewImages,
      autoResize,
      removeImage,
    };
  },
};
</script>

<style lang="scss">
.windows {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  .icon {
    width: 4rem;
    height: 4rem;
    cursor: pointer;
    position: fixed;
    right: 1%;
    bottom: 1%;
    z-index: 2;
    &.active_icon {
      z-index: -1;
    }
  }
  .window_boxs {
    width: 100%;
    height: 100%;
    background: rgba($color: #000000, $alpha: 0.5);
    display: flex;
    justify-content: center;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    visibility: hidden;
    opacity: 0;
    -webkit-transition: opacity 0.5s, visibility 0s linear 0.5s;
    transition: opacity 0.5s, visibility 0s linear 0.5s;
    z-index: -1;
    &.active_windows {
      visibility: visible;
      opacity: 1;
      -webkit-transition-delay: 0s;
      transition-delay: 0s;
      z-index: 1;
    }
    .window_box {
      width: 100%;
      max-width: 70rem;
      background: #fff;
      margin: 3.2rem 0;
      padding: 3.2rem 2.4rem;
      .box_title {
        display: flex;
        justify-content: space-between;
        margin: 0 0 4rem;
        h1 {
          font-size: 2.4rem;
          font-weight: bold;
        }
        button {
          padding: 0 1.5rem 0 0;
          border: 0;
          background: transparent;
          cursor: pointer;
          display: flex;
          align-items: flex-start;
        }
      }
      .box_inputs {
        padding: 0 2.4rem;
        .input_text,
        .input_select,
        .input_textarea {
          display: flex;
          flex-direction: column-reverse;
          align-items: flex-start;
          margin: 0 0 3.2rem;
          input,
          select,
          textarea {
            &:focus {
              outline: none;
            }
          }
          label {
            font-size: 1.6rem;
            color: gray;
            margin: 0 0 1rem;
            span {
              color: red;
              margin: 0 0.5rem 0 0;
            }
            small {
              margin: 0 0 0 0.5rem;
              font-size: 1.4rem;
              color: #c8c8c8;
            }
          }
        }
        .input_select {
          .select_arr {
            width: 100%;
            position: relative;
            &::after {
              display: block;
              width: 1.2rem;
              height: 1.2rem;
              content: "﹀";
              position: absolute;
              right: 1.5rem;
              top: 50%;
              transform: translate(0, -50%);
              pointer-events: none;
              font-size: 12px;
              color: #c8c8c8;
              z-index: 2;
            }
            select {
              width: 100%;
              padding: 0.8rem;
              border: 1px solid #c8c8c8;
              border-radius: 0.2rem;
              font-size: 1.6rem;
              appearance: none;
              position: relative;

              &:invalid {
                color: #c8c8c8;
              }
              option {
                color: #000000;
              }
            }
          }
        }
        .input_text {
          input[type="text"] {
            width: 100%;
            padding: 0.8rem;
            border: 1px solid #c8c8c8;
            border-radius: 0.2rem;
            font-size: 1.6rem;
            &::placeholder {
              color: #c8c8c8; /* 修改顏色 */
              opacity: 1; /* 確保顏色不會被透明度影響 */
            }
          }
        }
        .input_textarea {
          textarea {
            width: 100%;
            height: 3.9rem;
            padding: 0.8rem;
            border: 1px solid #c8c8c8;
            border-radius: 0.2rem;
            font-size: 1.6rem;
            resize: none;
            overflow: hidden;
            &::placeholder {
              color: #c8c8c8; /* 修改顏色 */
              opacity: 1; /* 確保顏色不會被透明度影響 */
            }
          }
        }
        .input_file {
          display: flex;
          flex-direction: column;
          align-items: flex-start;
          h2 {
            font-size: 1.6rem;
            color: gray;
            margin: 0 0 1rem;
            small {
              margin: 0 0 0 0.5rem;
              font-size: 1.4rem;
              color: #c8c8c8;
            }
          }
          .input_fileimages {
            width: 100%;
            display: flex;
            margin: 0 0 1rem;
            .image_previews {
              display: flex;
              .image_container {
                position: relative;
                margin: 0 1rem 0 0;
                .image_preview {
                  width: 13rem;
                  height: 13rem;
                  object-fit: cover;
                  border-radius: 8px;
                  border: 1px solid #c8c8c8;
                  transition: opacity 0.3s ease;
                }
                .overlay {
                  position: absolute;
                  top: 0;
                  left: 0;
                  right: 0;
                  bottom: 0;
                  background: rgba(0, 0, 0, 0.5); /* 半透明黑色背景 */
                  opacity: 0; /* 默認隱藏 */
                  display: flex;
                  justify-content: center;
                  align-items: center;
                  transition: opacity 0.3s ease;
                  .overlay_icon {
                    background: transparent;
                    border: none;
                    color: white;
                    font-size: 2rem;
                    cursor: pointer;
                    margin: 0 0.5rem;
                    width: 5rem;
                    height: 5rem;
                  }
                }

                &:hover {
                  .overlay {
                    opacity: 1; /* hover 時顯示 */
                  }
                  .image_preview {
                    opacity: 0.7; /* hover 時稍微變暗 */
                  }
                }
              }
            }
            input[type="file"] {
              display: none;
              & + label {
                display: flex;
                align-items: center;
                justify-content: center;
                width: 100%;
                max-width: 13rem;
                height: 13rem;
                border: 2px dashed #ccc;
                border-radius: 8px;
                cursor: pointer;
                .fileimages-plus {
                  font-size: 3.2rem;
                  color: #888;
                }
              }
            }
          }
          p {
            font-size: 1.4rem;
            color: gray;
            span {
              color: green;
            }
          }
        }
        #submit {
          width: 100%;
          padding: 1rem 0;
          margin: 2rem 0 0;
          background: #0d6efd;
          border: 0;
          font-size: 1.6rem;
          color: #fff;
          cursor: pointer;
        }
      }
    }
  }
}
</style>
