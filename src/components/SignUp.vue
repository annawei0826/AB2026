<template>
  <div class="sec signup-section" id="sec4">
    <div class="mid_content" data-aos="fade-up">
      <div class="article_title_wrap">
        <div class="article_title">
          {{ infoData.title }}
        </div>
      </div>

      <div class="form_container">
        <div class="form_wrap">
          <form name="data_form" id="data_form" @submit.prevent>
            <div class="form_item">
              <div class="title">姓名<span>必填</span></div>
              <div class="input">
                <input type="text" name="Name" class="single_line" placeholder="姓名">
              </div>
            </div>

            <div class="form_item">
              <div class="title">行動電話<span>必填</span></div>
              <div class="input">
                <input type="text" name="Mobile" class="single_line" placeholder="行動電話">
              </div>
            </div>

            <div class="form_item">
              <div class="title">年齡<span>必填</span></div>
              <div class="input_select">
                <select name="Age">
                  <option value="" selected>請選擇</option>
                  <option value="20歲以下">20歲以下</option>
                  <option value="20-25">20-25歲</option>
                  <option value="26-35">26-35歲</option>
                  <option value="36-45">36-45歲</option>
                  <option value="46-55">46-55歲</option>
                  <option value="56-65">56-65歲</option>
                  <option value="66歲以上">66歲以上</option>
                </select>
              </div>
            </div>

            <div class="form_item">
              <div class="title">電子信箱<span>必填</span></div>
              <div class="input">
                <input type="text" name="Email" class="single_line" placeholder="Email">
              </div>
            </div>

            <div class="form_item">
              <div class="title">公司單位<span>必填</span></div>
              <div class="input">
                <input type="text" name="Company_name" class="single_line" placeholder="公司單位">
              </div>
            </div>

            <div class="form_item">
              <div class="title">產業別<span>必填</span></div>
              <div class="input_select">
                <select name="Industry">
                  <option value="" selected>請選擇</option>
                  <option value="學生">學生</option>
                  <option value="農、林、漁、牧業">農、林、漁、牧業</option>
                  <option value="教育服務業">教育服務業</option>
                  <option value="醫療保健及社會工作服務業">醫療保健及社會工作服務業</option>
                  <option value="製造業">製造業</option>
                  <option value="服務業">服務業</option>
                  <option value="公務人員">公務人員</option>
                  <option value="自營商">自營商</option>
                  <option value="家管">家管</option>
                  <option value="自由工作者">自由工作者</option>
                  <option value="其他">其他</option>
                </select>
              </div>
            </div>

            <div class="form_item">
              <div class="title">職稱<span>必填</span></div>
              <div class="input">
                <input type="text" name="Job_title" class="single_line" placeholder="職稱">
              </div>
            </div>

            <div class="form_item">
              <div class="title">報名場次<span>必填</span></div>
              <div class="input_select">
                <select name="sessions">
                  <option value="" selected>請選擇</option>
                  <option value="1/6（二）臺中場">1/6（二）臺中場</option>
                  <option value="1/7（三）臺北場">1/7（三）臺北場</option>
                  <option value="1/8（四）高雄場">1/8（四）高雄場</option>
                </select>
              </div>
            </div>

            <div class="form_item form_item_source">
              <div class="title">活動資料來源<span>必填</span></div>
              <div class="input_select">
                <select name="source">
                  <option value="" selected>請選擇</option>
                  <option value="今周刊">今周刊</option>
                  <option value="聯博投信">聯博投信</option>
                  <option value="券商宣傳">券商宣傳</option>
                  <option value="Facebook">Facebook</option>
                  <option value="其他">其他</option>
                </select>
              </div>
            </div>

            <div class="field">
              <div class="terms-box">
                <p class="terms-content">
                  謹依個人資料保護法第8條規定告知以下事項:
                </p>
                <p class="terms-content">
                  您為本次活動的參與者(以下簡稱參與人)。參與人所填寫的資料將會提供予今周刊。請確認所有參與人在勾選同意前已詳閱並同意以下條款:今周刊基於客戶管理、統計及調查分析、行銷及其他合於營業登記項目或章程所定業務需要或其他法令所准許之特定目的,向參與人蒐集前述個人資料,並管理維護及處理利用,其得就該等個人資料用於寄送出版物、提供各項服務或活動訊息與調查分析使用。參與人得向活動執行單位請求查閱、提供複本、更正或補充個人資訊,及請求刪除或停止處理利用。若參與人填寫資料不完整時,可能會影響參與人獲得今周刊所提供之服務或各項訊息通知與活動之權益。
                </p>
                <p class="terms-content">
                  活動全程免費,主辦單位保有修改活動內容及審核出席的權利。
                </p>
                
                <div class="checkbox-wrapper">
                  <label class="checkbox-label">
                    <input type="checkbox" class="ckBox" name="Is_agree" id="Is_agree">
                    <span class="checkbox-text">我已詳閱並同意個資告知事項</span>
                  </label>
                </div>
              </div>
            </div>

            <div class="btn_wrap">
              <button 
                type="button" 
                class="submit submit-image-btn" 
                id="btn"
                @click="checkVal" 
                :disabled="isSubmitting"
              >
                <img src="@/assets/image/SignUp_button.png" alt="確認">
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

defineProps({
  infoData: {
    type: Object,
    default: () => ({})
  }
});

const isSubmitting = ref(false);

const checkVal = () => {
  const Name = document.querySelector("input[name='Name']").value;
  let phone = document.querySelector("input[name='Mobile']").value;
  phone = phone.replace(/[^0-9.]/g, '');
  const Email = document.querySelector("input[name='Email']").value;
  const Industry_type = document.querySelector("[name='Industry']").value;
  const Company = document.querySelector("input[name='Company_name']").value;
  const title = document.querySelector("input[name='Job_title']").value;
  const age = document.querySelector("[name='Age']").value;
  const sessions = document.querySelector("[name='sessions']").value;
  const source = document.querySelector("[name='source']").value;

  let error = false;
  let txt = '錯誤:';

  if (Name == "") {
    txt = txt + ' \n✻ 請輸入姓名';
    if (!error) {
      document.querySelector("input[type='text'][name='Name']").focus();
    }
    error = true;
  }

  if (phone == "") {
    txt = txt + '\n✻ 請輸入聯絡手機';
    if (!error) {
      document.querySelector("input[name='Mobile']").focus();
    }
    error = true;
  } else {
    const MobileReg = /^(09)[0-9]{8}$/;
    if (!MobileReg.test(phone)) {
      txt = txt + '\n✻ 手機格式錯誤,格式為十個數字 ex 0912345678';
      if (!error) {
        document.querySelector("input[name='Mobile']").focus();
      }
      error = true;
    }
  }

  if (age == "") {
    txt = txt + ' \n✻ 請選擇年齡選項';
    if (!error) {
      document.querySelector("[name='Age']").focus();
    }
    error = true;
  }

  if (Email == "") {
    txt = txt + '\n✻ 請輸入Email';
    if (!error) {
      document.querySelector("input[name='Email']").focus();
    }
    error = true;
  } else if (!ValidateEmail(Email)) {
    txt = txt + '\n✻ Email信箱格式不正確,格式 ex example@mail.com';
    if (!error) {
      document.querySelector("input[name='Email']").focus();
    }
    error = true;
  }

  if (Company == "") {
    txt = txt + ' \n✻ 請輸入公司單位';
    if (!error) {
      document.querySelector("input[type='text'][name='Company_name']").focus();
    }
    error = true;
  }

  if (Industry_type == "") {
    txt = txt + ' \n✻ 請選擇產業別';
    if (!error) {
      document.querySelector("[name='Industry']").focus();
    }
    error = true;
  }

  if (title == "") {
    txt = txt + ' \n✻ 請輸入職稱';
    if (!error) {
      document.querySelector("input[type='text'][name='Job_title']").focus();
    }
    error = true;
  }

  if (sessions == "") {
    txt = txt + ' \n✻ 請選擇報名場次';
    if (!error) {
      document.querySelector("[name='sessions']").focus();
    }
    error = true;
  }

  if (source == "") {
    txt = txt + ' \n✻ 請選擇活動資料來源';
    if (!error) {
      document.querySelector("[name='source']").focus();
    }
    error = true;
  }

  if (document.querySelector("#Is_agree").checked == false) {
    txt = txt + '\n✻ 請勾選同意條款';
    if (!error) {
      document.querySelector("#Is_agree").focus();
    }
    error = true;
  }

  if (error) {
    alert(txt);
    return false;
  }

  document.querySelector('#btn').disabled = true;
  sign_up();
};

const ValidateEmail = (Email) => {
  const validRegexvar = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
  if (validRegexvar.test(Email)) {
    return true;
  } else {
    return false;
  }
};

const sign_up = async () => {
  isSubmitting.value = true;
  
  const form = document.querySelector('#data_form');
  const data = new FormData(form);

  try {
    const response = await axios.post('https://events.businesstoday.com.tw/backend/AB2026/sign_up', data, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    });

    if (response.data.bIsSuccess == 0) {
      alert(response.data.sError);
      document.querySelector('#btn').disabled = false;
      isSubmitting.value = false;
      return;
    } else {
      alert(response.data.successMsg || "感謝您的報名,本活動報名採審核制,後續將以簡訊、e-mail通知您是否報名成功,如未收到代表未報名成功");
      document.querySelector('#data_form').reset();
      
      const sec3 = document.querySelector('#sec3');
      if (sec3) sec3.scrollIntoView({ behavior: 'smooth' });
      
      document.querySelector('#btn').disabled = false;
      isSubmitting.value = false;
    }
  } catch (error) {
    console.error(error);
    alert("系統忙碌中,請稍後再試!");
    document.querySelector('#btn').disabled = false;
    isSubmitting.value = false;
  }
};
</script>

<style scoped>

* {
  box-sizing: border-box;
}

.signup-section {
  background-image: url('@/assets/image/bg-blue2.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;

}

.article_title_wrap {
  position: relative;
  z-index: 5;
  clear: both;
}

.form_container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  background-color: #ffffff;
  border-radius: 20px;
  padding: 40px;
  box-sizing: border-box;
  clear: both;
  position: relative;
  z-index: 2;
}

.single_line,
select {
  width: 100%;
  height: 44px;
  min-height: 44px;
  
  padding: 10px 14px;
  margin: 0;
  
  font-family: inherit;
  font-size: 15px;
  font-weight: 400;
  color: #333333;
  line-height: 1.5;
  
  border: 1px solid #d0d0d0;
  border-radius: 8px;
  
  background-color: #ffffff;
  
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  outline: none;
  
  box-sizing: border-box;
  transition: border-color 0.2s ease;
  
  display: flex;
  align-items: center;
}

.single_line::placeholder {
  color: #999999;
  font-size: 15px;
  font-weight: 400;
  opacity: 1;
}

.single_line::-webkit-input-placeholder {
  color: #999999;
  font-size: 15px;
}

.single_line::-moz-placeholder {
  color: #999999;
  font-size: 15px;
}

.single_line:-ms-input-placeholder {
  color: #999999;
  font-size: 15px;
}

select {
  cursor: pointer;
  padding-right: 40px;
  
  background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23666666' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
  background-repeat: no-repeat;
  background-position: right 14px center;
  background-size: 16px;
}

select option[value=""] {
  color: #999999;
}

select[value=""] {
  color: #999999;
}

select:not([value=""]) {
  color: #333333;
}

select option {
  color: #333333;
  padding: 8px;
}

.single_line:focus,
select:focus {
  border-color: #0066cc;
  outline: none;
}

.single_line:hover,
select:hover {
  border-color: #0066cc;
}
.single_line:disabled,
select:disabled {
  background-color: #f5f5f5;
  color: #999999;
  cursor: not-allowed;
  opacity: 0.6;
}

.form_item_source {
  width: 98%;
  flex-basis: 100%;
  max-width: 100%;
}

.field {
  width: 98%;
  max-width: 100%;
  margin-top: 20px;
}

.terms-box {
  background-color: #d9d9d9;
  border-radius: 16px;
  padding: 24px 28px;
  margin-bottom: 20px;
}

.terms-content {
  font-size: 13px;
  line-height: 1.9;
  color: #000000;
  margin: 0 0 14px 0;
  text-align: justify;
  font-weight: 400;
  letter-spacing: 0.3px;
}

.terms-content:last-child {
  margin-bottom: 0;
}

.checkbox-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
}

.checkbox-label {
  display: flex;
  align-items: center;
  cursor: pointer;
  user-select: none;
}

.ckBox {
  width: 18px;
  height: 18px;
  margin: 0;
  margin-right: 10px;
  cursor: pointer;
  flex-shrink: 0;
}

.checkbox-text {
  font-size: 16px;
  color: #333333;
  font-weight: 700;
  line-height: 1.5;
}

.submit-image-btn {
  display: inline-block;
  width: 190px;
  height: auto;
  border: none;
  background: none;
  padding: 0;
  cursor: pointer;
  vertical-align: middle;
  transition: opacity 0.3s ease;
}

.submit-image-btn:hover:not(:disabled) {
  opacity: 0.5;
}

.submit-image-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.submit-image-btn img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: contain;
}

@media screen and (max-width: 768px) {
  .signup-section {
    padding-top: 40px;
    padding-bottom: 40px;
  }

  .form_container {
    padding: 30px 20px;
    border-radius: 16px;
  }

  .single_line,
  select {
    height: 42px;
    min-height: 42px;
    font-size: 14px;
    padding: 9px 12px;
  }

  .single_line::placeholder,
  .single_line::-webkit-input-placeholder,
  .single_line::-moz-placeholder {
    font-size: 14px;
  }

  select {
    padding-right: 36px;
    background-size: 14px;
  }
  
  .form_item_source {
    width: 100%;
  }
  
  .field {
    width: 100%;
  }
  
  .terms-box {
    padding: 20px 22px;
    border-radius: 14px;
  }
  
  .terms-content {
    font-size: 12px;
    line-height: 1.85;
    margin-bottom: 12px;
  }
  
  .checkbox-text {
    font-size: 14px;
  }
  
  .ckBox {
    width: 16px;
    height: 16px;
  }
}

@media screen and (max-width: 480px) {


  .form_container {
    padding: 20px 15px;
    border-radius: 12px;
  }

  .single_line,
  select {
    height: 40px;
    min-height: 40px;
    font-size: 14px;
    padding: 8px 12px;
  }

  .single_line::placeholder,
  .single_line::-webkit-input-placeholder,
  .single_line::-moz-placeholder {
    font-size: 14px;
  }

  select {
    padding-right: 34px;
    background-size: 13px;
  }
  
  .submit-image-btn {
    width: 130px;
  }
}


@supports (-webkit-touch-callout: none) {
  .single_line,
  select {
    -webkit-appearance: none;
  }
  
  select {
    background-position: right 14px center;
  }
}
</style>