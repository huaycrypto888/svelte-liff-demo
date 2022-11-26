<script>
  import liff from "@line/liff";
  import Geolocation from "svelte-geolocation";
  import moment from "moment";
  import LIFFInspectorPlugin from "@line/liff-inspector";
  import { LiffMockPlugin } from "@line/liff-mock";

  import { onMount } from "svelte";

  liff.use(new LiffMockPlugin());
  // liff.use(
  //   new LIFFInspectorPlugin({
  //     // origin: import.meta.env.VITE_LI_ORIGIN
  //     //   ? import.meta.env.VITE_LI_ORIGIN
  //     //   : "ws://localhost:9222",
  //   })
  // );


  const agent = navigator.userAgent;
  let coords = [0, 0];
  let profile = {};
  let errorMessage = "";
  async function init() {
    let mock = agent.includes("LIFF") ? false : true;
    return await liff.init({
      liffId: import.meta.env.VITE_LIFF_ID,
      mock,
    });
  }

  
  let promise = init().then(async () => {
    profile = await liff.getProfile();
  });

  const setLiffMock = () => {
    liff.$mock.set((p) => ({
      ...p,
      getProfile: { displayName: "Cony *Mocked", userId: "1234" },
    }));
  };

  const closeWindow = () => {
    if (!liff.isInClient()) {
      window.alert(errorMessage);
    } else {
      liff.closeWindow();
    }
  };

 
    async function checkfriend() {
      const friend = await liff.getFriendship();
      return friend.friendFlag;
    }



    

  async function addChild(typeMarket) {
    let urlparam = new URLSearchParams(window.location.search);
    let root = urlparam.get("root")
    let child = profile.userId

    const response = await fetch('https://chickenad.vercel.app/api/customers', {

      method: 'POST',
      body: JSON.stringify({ root, child, typeMarket })
    });
  }

async function shareTarket(typeMarket) {
    const isFriend =  checkfriend();
    if(!isFriend)
    {
      window.alert('not friend');
      window.location = "https://line.me/R/ti/p/@701fnoik";
    }
    else{
      addChild(typeMarket);
    }
    const message = liff.shareTargetPicker([
      {
        type: "flex",
        altText: "this is a flex message",
        contents:{
          "type": "bubble",
          "hero": {
            "type": "image",
            "url": "https://i.ibb.co/V2bPc0Q/logo-tanggai.jpg",
            "size": "full",
            "aspectRatio": "20:13",
            "aspectMode": "cover"
          },
          "body": {
            "type": "box",
            "layout": "vertical",
            "contents": [
              {
                "type": "text",
                "text": "แทงไก่ ออโต้",
                "weight": "bold",
                "size": "xl",
                "align": "center",
              },
              {
                "type": "box",
                "layout": "baseline",
                "margin": "md",
                "contents": [
                  {
                    "type": "icon",
                    "size": "sm",
                    "url": "https://scdn.line-apps.com/n/channel_devcenter/img/fx/review_gold_star_28.png"
                  },
                  {
                    "type": "icon",
                    "size": "sm",
                    "url": "https://scdn.line-apps.com/n/channel_devcenter/img/fx/review_gold_star_28.png"
                  },
                  {
                    "type": "icon",
                    "size": "sm",
                    "url": "https://scdn.line-apps.com/n/channel_devcenter/img/fx/review_gold_star_28.png"
                  },
                  {
                    "type": "icon",
                    "size": "sm",
                    "url": "https://scdn.line-apps.com/n/channel_devcenter/img/fx/review_gold_star_28.png"
                  },
                  {
                    "type": "icon",
                    "size": "sm",
                    "url": "https://scdn.line-apps.com/n/channel_devcenter/img/fx/review_gold_star_28.png"
                  },
                  {
                    "type": "text",
                    "text": "การันตีระดับ 5 ดาว",
                    "size": "sm",
                    "color": "#999999",
                    "margin": "md",
                    "flex": 0
                  }
                ]
              },
              
            ]
          },
          "footer": {
            "type": "box",
            "layout": "vertical",
            "spacing": "sm",
            "contents": [
              {
                "type": "text",
                "text": "กลุ่มลับ VIP Exclusive",
                "weight": "bold",
                "size": "xl",
                "align": "center",
              },
              {
                "type": "button",
                "style": "primary",
                "height": "sm",
                "action": {
                  "type": "uri",
                  "label": "แชร์ เพื่อรับรายได้ 30% จากยอดเสีย",
                  "uri": "https://liff.line.me/1657611539-z41ew0PW/?root=" + profile.userId
                },
                "color": "#3482FA"
              },
              {
                "type": "button",
                "style": "primary",
                "height": "sm",
                "action": {
                  "type": "uri",
                  "label": "แชร์ เพื่อรับรายได้ 5% จากยอดเล่น",
                  "uri": "https://liff.line.me/1657611539-z41ew0PW/?root=" + profile.userId
                },
                "color": "#3482FA"
              },
              {
                "type": "button",
                "style": "primary",
                "height": "sm",
                "action": {
                  "type": "uri",
                  "label": "แชร์ เพื่อรับรายได้ 100 บาท/คน",
                  "uri": "https://liff.line.me/1657611539-z41ew0PW/?root=" + profile.userId
                },
                "color": "#3482FA"
              },
              {
                "type": "button",
                "style": "primary",
                "height": "sm",
                "action": {
                  "type": "uri",
                  "label": "แชร์ เพื่อรับรายได้ 2% จากยอดฝาก",
                  "uri": "https://liff.line.me/1657611539-z41ew0PW/?root=" + profile.userId
                },
                "color": "#3482FA"
              },
              {
                "type": "box",
                "layout": "vertical",
                "contents": [],
                "margin": "sm"
              }
            ],
            "flex": 0
          }
        },
      }]);
  };



  async function joinGroup(groupType) {
    const isFriend =  checkfriend();
    if(groupType == 1)
    {
      window.alert('กลุ่มนี้เข้าแล้วอยู่ดีมีแฮง อยู่แดงมีฮี');
      window.location = "https://line.me/R/ti/p/@701fnoik";
    }
    else if(groupType == 2){
      window.alert('กลุ่มนี้ใบ้หวยแม่นที่สุดในดาวอังคารแล้วค่ะ');
      window.location = "https://line.me/R/ti/p/@701fnoik";
    }
    else if(groupType == 3){
      window.alert('กลุ่มนี้ทำนายฝันแม่นมากกก แม่นจนนึกว่าฝันไป');
      window.location = "https://line.me/R/ti/p/@701fnoik";
    }
    else if(groupType == 4){
      window.alert('กลุ่มนี้แนะนำเทคนิคดีๆในการเล่นพนัน ให้ปังปูริ เย่ เย่');
      window.location = "https://line.me/R/ti/p/@701fnoik";
    }
    else if(groupType == 5){
      window.alert('กลุ่มนี้แฉกลโกงต่างๆที่น้องมินนี่ได้ประสบพบเจอมาค่ะ');
      window.location = "https://line.me/R/ti/p/@701fnoik";
    }
  };


// async function sendMessage() {
//   if(!isFriend)
//   {
//     window.alert('not friend');
//     window.location = "https://line.me/R/ti/p/@701fnoik";
//   }
//   else{
//     window.alert("รายได้ของคุณ = ??")
//   }
// };





  onMount(async () => {
    if (!liff.isInClient()) {
      errorMessage =
        "The App is not opened in LINE, LIFF function will not work and will use Mocked User Info \n To open in line, use the QR below";
      // Run Login and Retrieve mock profile
      setLiffMock();
      liff.login();
      profile = await liff.getProfile();
    }
  });
</script>

<!-- <Geolocation getPosition bind:coords /> -->
<main>
  <!-- <h1>แทงไก่ ออโต้</h1>
  <h2>การันตี ชื่อนี้ไม่มีเสีย</h2> -->
  {#await promise}
    <p>LIFF init...</p>
  {:then}
  <a href="https://line.me/R/ti/p/@701fnoik"><img src="https://i.ibb.co/FgxhQFB/logo-tanggai-00001.jpg" alt="แทงไก่" border="0" /></a>
 <br>
  <button class="button-91" on:click={() => shareTarket(1)}>แชร์ เพื่อรับรายได้ 30% จากยอดเสีย</button>
  <br>
  <button class="button-91" on:click={() => shareTarket(2)}>แชร์ เพื่อรับรายได้ 5% จากยอดเล่น</button>
  <br>
  <button class="button-91" on:click={() => shareTarket(3)}>แชร์ เพื่อรับรายได้ 100 บาท/คน</button>
  <br>
  <button class="button-91" on:click={() => shareTarket(4)}>แชร์ เพื่อรับรายได้ 2% จากยอดฝาก</button>
  <!-- <button class="button-91" on:click={sendMessage}>ตรวจสอบรายได้ </button> -->


    <hr />
    <!-- <h3>ได้โปรด อ่านด้านล่าง หนูขอร้อง อิคึ..อิคึ..</h3>
    <div class="center" >สวัสดีค่ะคุณ <strong>{profile.displayName}</strong> <br> </div>
    <div>ทางเรามีบริการต่างๆ สิทธิพิเศษ แบบ Exclusive VIP ขอบอกว่างานนี้ฟรีไม่มีค่าใช้จ่ายนะค่ะ</div> -->
    <!-- <ul>
      <li>
        <span class=".center" >สวัสดีค่ะคุณ <strong>{profile.displayName}</strong> <br> </span>
        <span class=".center" >สวัสดีค่ะคุณ <strong>{profile.displayName}</strong> <br> ทางเรามีบริการต่างๆ สิทธิพิเศษ แบบ Exclusive VIP<br>ไม่มีค่าใช้จ่ายเลยนะค่ะ </span>
      </li>
    </ul> -->
    <!-- <div>กลุ่มนี้เข้าแล้วอยู่ดีมีแฮง อยู่แดงมีฮี</div> -->
    <button class="button-70" on:click={() => joinGroup(1)}>หัวกรวย ใบ้หวย รวยชิปหาย</button>

    <button class="button-70" on:click={() => joinGroup(6)}>เลขที่ไม่ออก ไม่อยากพลาด อย่าซื้อตัวนี้</button>

    <button class="button-70" on:click={() => joinGroup(2)}>ทำนายฝัน แม่นเหมือนจับวาง</button>

    <button class="button-70" on:click={() => joinGroup(3)}>ดูดวง ลิตขิตฟ้าหรือจะสู้ กุรู้ก่อน</button>

    <button class="button-70" on:click={() => joinGroup(4)}>เทคนิค เป็นต่อ เล่นให้เฟี้ยว เลี้ยวให้รอด</button>

    <button class="button-70" on:click={() => joinGroup(5)}>แฉ กลโกง ไม่อยากหมดตูดเพราะถูกโกง</button>

  
    <h4 class="error">{errorMessage}</h4>

    <!-- <hr /> -->
    <!-- <h3>LIFF Info</h3>
    <ul>
      <li>
        <strong>LIFF Browser</strong>
        :<span>{liff.isInClient()}</span>
      </li>
      <li>
        <strong>Login Status</strong>
        :<span>{liff.isLoggedIn()}</span>
      </li>
      <li>
        <strong>Language</strong>
        :<span>{liff.getLanguage()}</span>
      </li>
      <li>
        <strong>OS</strong>
        :<span>{liff.getOS()}</span>
      </li>
      <li>
        <strong>LIFF Ver</strong>
        :<span>{liff.getVersion()}</span>
      </li>
      <li>
        <strong>LINE Ver</strong>
        :<span>{liff.getLineVersion()}</span>
      </li>
    </ul> -->
    <hr />
    <h3>เพิ่มเพื่อนผ่าน QR Code</h3>
    <img src="./liff-qr.png" alt="" />
  {:catch e}
    <p>LIFF init failed.</p>
    <p><code>{`${e}`}</code></p>
  {/await}
</main>

<style>
  main {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  /* .center {
  text-align: center;
  border: 3px solid green;
  } */
  .error {
    color: red;
  }
  /* ul {
    list-style-type: none;
    text-align: left;
    margin: 0;
    padding: 0;
  }
  li {
    padding: 5px;
    border: 1px solid #dddddd;
  }
  li span {
    margin: 5px;
  } */


  .button-91 {
  color: #fff;
  padding: 15px 25px;
  background-color: #38D2D2;
  background-image: radial-gradient(93% 87% at 87% 89%, rgba(0, 0, 0, 0.23) 0%, transparent 86.18%), radial-gradient(66% 66% at 26% 20%, rgba(255, 255, 255, 0.55) 0%, rgba(255, 255, 255, 0) 69.79%, rgba(255, 255, 255, 0) 100%);
  box-shadow: inset -3px -3px 9px rgba(255, 255, 255, 0.25), inset 0px 3px 9px rgba(255, 255, 255, 0.3), inset 0px 1px 1px rgba(255, 255, 255, 0.6), inset 0px -8px 36px rgba(0, 0, 0, 0.3), inset 0px 1px 5px rgba(255, 255, 255, 0.6), 2px 19px 31px rgba(0, 0, 0, 0.2);
  border-radius: 14px;
  font-weight: bold;
  font-size: 16px;

  border: 0;

  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;

  cursor: pointer;

  width: 320px;
  margin-bottom: 4px;
}


/* CSS */
.button-70 {
  background-image: linear-gradient(#0dccea, #0d70ea);
  border: 0;
  border-radius: 4px;
  box-shadow: rgba(0, 0, 0, .3) 0 5px 15px;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  font-family: Montserrat,sans-serif;
  font-size: .9em;
  margin: 5px;
  padding: 10px 15px;
  text-align: center;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  margin-bottom: 5px;
  width: 300px;
}



/* CSS */
/* .button-88 {
  display: flex;
  align-items: center;
  font-family: inherit;
  font-weight: 500;
  font-size: 16px;
  padding: 0.7em 1.4em 0.7em 1.1em;
  color: white;
  background: #ad5389;
  background: linear-gradient(0deg, rgba(20,167,62,1) 0%, rgba(102,247,113,1) 100%);
  border: none;
  box-shadow: 0 0.7em 1.5em -0.5em #14a73e98;
  letter-spacing: 0.05em;
  border-radius: 20em;
  cursor: pointer;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button-88:hover {
  box-shadow: 0 0.5em 1.5em -0.5em #14a73e98;
}

.button-88:active {
  box-shadow: 0 0.3em 1em -0.5em #14a73e98;
} */
</style>



<!-- 
async function testget() {
  const response = await fetch('https://chickenad.vercel.app/api/customers');
  let total = "x";
  total = await response.json();
  window.alert(total.message)
}


async function testput() {
  const response = await fetch('https://chickenad.vercel.app/api/customers/+server.js');
  let total = "x";
  total = await response.json();
  window.alert(total.message)
}


  async function testpost() {

    const response = await fetch('http://localhost:5173/api/customers?root=1&child=2', {
      method: 'POST'
    });
  
  }

async function testpost() {
  let root = "root"
  let child = "child"
  const response = await fetch('https://chickenad.vercel.app/api/customers', {
    method: 'POST',
    body: JSON.stringify({ root, child })
  });
  let total = "x"Ź
  total = await response.json();
  window.alert(total.message)
} -->

<!-- async function addChild() {
  let urlparam = new URLSearchParams(window.location.search);

  window.alert(urlparam.get("root"))
  window.alert(profile.userId)

  const response = await fetch('https://chickenad.vercel.app/api/customers', {

    method: 'POST',
    body: JSON.stringify({ root, child })
  });
  // const response = await fetch('http://localhost:5173/api/customers?root=1&child=2', {
  //   method: 'POST'
  // });
  // let total = await response.json();

} -->
