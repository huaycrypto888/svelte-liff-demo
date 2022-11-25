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



    

  async function addChild() {
    let urlparam = new URLSearchParams(window.location.search);
    let root = 1
    let child = 2
    // window.alert(root)
    // window.alert(profile.userId)
    // let root = "123"
    // let child = "456"
    const response = await fetch('https://chickenad.vercel.app/api/customers', {
    // const response = await fetch('http://localhost:5173/api/customers', {
      method: 'POST',
      body: JSON.stringify({ root, child })
    });
    // const response = await fetch('http://localhost:5173/api/customers?root=1&child=2', {
    //   method: 'POST'
    // });
    let total = await response.json();
    window.alert(total)
  }


  const shareTarket = () => {
    const isFriend =  checkfriend();
    if(!isFriend)
    {
      window.alert('not friend');
      window.location = "https://line.me/R/ti/p/@701fnoik";
    }
    else{
      addChild();
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
            "aspectMode": "cover",
            "action": {
              "type": "uri",
              "uri": "https://line.me/R/ti/p/@701fnoik?param=11111"
            }
          },
          "body": {
            "type": "box",
            "layout": "vertical",
            "contents": [
              {
                "type": "text",
                "text": "แทงไก่ ออโต้",
                "weight": "bold",
                "size": "xl"
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
              // {
              //   "type": "button",
              //   "style": "primary",
              //   "height": "sm",
              //   "action": {
              //     "type": "uri",
              //     "label": "สมัคร ตอนนี้รับโบนัส X2",
              //     "uri": "https://line.me/R/ti/p/@701fnoik?root=" + profile.userId
              //   }
              // },
              {
                "type": "button",
                "style": "primary",
                "height": "sm",
                "action": {
                  "type": "uri",
                  "label": "แชร์ เพื่อรับรายได้ 30%",
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



async function sendMessage() {
  if(!isFriend)
  {
    window.alert('not friend');
    window.location = "https://line.me/R/ti/p/@701fnoik";
  }
  else{
    window.alert("รายได้ของคุณ = ??")
  }
};

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
  <h1>แทงไก่ ออโต้</h1>
  <h2>การันตี ชื่อนี้ไม่มีเสีย</h2>
  {#await promise}
    <p>LIFF init...</p>
  {:then}
  <a href="https://line.me/R/ti/p/@701fnoik"><img src="https://i.ibb.co/FgxhQFB/logo-tanggai-00001.jpg" alt="แทงไก่" border="0" /></a>
 <br>
  <button class="button-91" on:click={(shareTarket)}>แชร์เพื่อรับรายได้</button>
  <button class="button-91" on:click={sendMessage}>ตรวจสอบรายได้ </button>


    <hr />
    <h3>User Info</h3>
    <ul>
      <li>
        <strong>User Name</strong>
        :<span>{profile.displayName}</span>
      </li>
    </ul>
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
  .error {
    color: red;
  }
  ul {
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
  }


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

}
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
