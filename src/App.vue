<template>
  <div>
    <h1 class='title'>ACTIV Ideas</h1>
    <div class='loading' v-if="loading">
      <img src="./assets/loader.gif" alt="loading" />
    </div>
    <div class='main-container'>
      <div class="box1 box" v-for="idea in ideas" :key="idea.id">
        <div class="content">
          <div class="image">
            <img :src="idea?.idea?.public?.image" :alt="idea?.nftId" class='ticker-logo' />
          </div>
          <div class="level">
            <p>{{ idea?.idea?.idea?.kind || 'unknow' }}</p>
          </div>
          <div class="text">
            <p class="name">{{ idea?.idea?.idea?.asset?.ticker }}</p>
            <p class="job_title">{{ idea?.name }}</p>
            <p class="job_discription">{{ idea?.description }}</p>
          </div>
          <div class="button">
            <div>
              <button class="message" type="button" @click="goToIPFSUrl(idea)">See IPFS
                content</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
//@ts-ignore
import { Options, Vue } from 'vue-class-component';

//@ts-ignore
import detectEthereumProvider from '@metamask/detect-provider';

//@ts-ignore
import IXilyACTIV from '@ixily/activ-web';

@Options({
  components: {},
})
export default class App extends Vue {

  ideas: any[] = [];
  loading: boolean = false;

  public async mounted() {
    try {

      this.loading = true;

      const ethereum: any = await detectEthereumProvider();

      await ethereum?.request({ method: 'eth_requestAccounts' });

      const chainIds: any = {
        goerli: 5,
        hardhat: 1337, //31337
        kovan: 42,
        mainnet: 1,
        rinkeby: 4,
        ropsten: 3,
        mumbai: 80001,
        sepolia: 11155111,
      };

      const getNetworkName = (nId: string) =>
        Object.keys(chainIds)?.find(
          (key: any) => chainIds[key]?.toString() === nId?.toString()
        ) || null;

      const defaultNetwork: any = ethereum?.networkVersion || null;
      const networkName: any = getNetworkName(defaultNetwork);

      const providerWorks = (window as any).ethereum;

      const isPublic = providerWorks === null ? true : false;

      const activ: IXilyACTIV = new IXilyACTIV({
        webProvider: ethereum,
        public: isPublic,
      });

      if (activ) {
        await activ.init({
          network: networkName,
          showLogsToDebug: true,
        })

        const ideas = await activ.getAllIdeas(1, 30);

        this.ideas = ideas?.data;
        this.loading = false;
      }

    } catch (err: any) {
      console.log('error', err.message);
    }
  };

  goToIPFSUrl(idea: any) {
    window.open(idea?.url, '_blank');
  }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Nunito:wght@300&display=swap");

* {
  margin: 0;
  padding: 0;
  font-family: "Nunito", sans-serif;
}

:root {
  --green: #58a497;
  --yellow: #e09449;
  --lightgreen1: #a4bdb7;
  --brown: #8b4448;
  --gray: gray;
  --lightgreen2: rgb(164, 189, 183, 0.5);
  --box: #ededed;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;

  background: #212121;
  margin-top: 50px;
  margin-bottom: 50px;
}

.loading {
  text-align: center;

  & img {
    height: 45px;
    width: 45px;
  }
}

.title {
  color: #ededed;
  text-align: center;
  margin-bottom: 20px;
}

.main-container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

.ticker-logo {
  height: 150px;
  width: 150px;
}

/* Content-1:Start */
.box {
  width: 350px;
  height: 350px;
  border-radius: 20px;
  padding: 10px;
  text-align: center;
  background: #ededed;
  margin: 5px;
}

.box1 {
  margin-top: 10px;
}

.content {
  margin: 15px 2px;
}

.image img {
  height: auto;
  width: 120px;
  border-radius: 50%;

  display: block;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 5px;
}

.level {
  font-size: 0.7em;
  background-color: rgb(164, 189, 183, 0.5);
  width: 50px;
  padding: 3px;
  border-radius: 5px;
  font-weight: bolder;
  letter-spacing: 1px;

  display: block;
  margin: 0px auto 10px;
}

.name {
  font-size: 1.25em;
  font-weight: bolder;
  letter-spacing: 1px;
}

.job_title {
  font-size: 0.65em;
  font-weight: bolder;
  color: gray;
  margin-top: -2px;
}

.job_discription {
  font-size: 0.7em;
  color: gray;
  margin: 10px 30px 20px;
}

.icons {
  margin: 0px 30px;
  font-size: 1.5em;
  display: flex;
  justify-content: space-around;
}

.icons button {
  width: fit-content;
  height: fit-content;
  border: none;
  font-size: 1em;
}

ion-icon:hover {
  color: #58a497;
  transition: 0.5s;
}

button {
  width: 130px;
  height: 40px;
  border-radius: 10px;
  font-weight: bolder;
  cursor: pointer;
}

.button {
  display: flex;
  justify-content: space-around;
  flex-direction: row;
  margin: 20px 30px 0px;
}

.button .message {
  background: #ededed;
  border: 2px solid #000;
}

.button .connect {
  background-color: #000;
  color: #ededed;
  border: none;
}

button.connect:hover {
  letter-spacing: 1px;
  transition: 0.5s;
}

button.message:hover {
  letter-spacing: 1px;
  transition: 0.5s;
  background: rgba(88, 164, 151, 0.5);
}

/* Content-1:End */

/* Content-2:Start */
.box2 {
  margin-top: 50px;
  margin-bottom: 10px;
}

.box2 img {
  width: 80px;
}

.row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0px 0px 10px;
}

h5 {
  font-weight: bolder;
  font-size: 1em;
}

.row p {
  font-size: 0.8em;
}

.box2 .text {
  text-align: left;
}

.box2 .text .name {
  font-size: 1.1em;
}

.box2 .text .job_title {
  font-size: 0.6em;
  margin-bottom: 10px;
}

.box2 .text .job_discription {
  margin: 0px;
}

.box2 .text .about {
  font-size: 0.9em;
  font-weight: bolder;
}

/* Content-2:End */

/* Media Queries */

@media screen and (max-width: 1200px) {
  .main-container {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media screen and (max-width: 900px) {
  .main-container {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media screen and (max-width: 600px) {
  .main-container {
    grid-template-columns: repeat(1, 1fr);
  }
}
</style>
