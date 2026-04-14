<template>
  <div class="app-wrapper">

    <!-- FLOATING BACKGROUND EMOJI (pure decoration) -->
    <div class="floaties" aria-hidden="true">
      <span class="floaty" style="--x:8%;  --delay:0s;   --dur:7s">🌸</span>
      <span class="floaty" style="--x:20%; --delay:1.2s; --dur:9s">✨</span>
      <span class="floaty" style="--x:35%; --delay:2.5s; --dur:6s">💐</span>
      <span class="floaty" style="--x:50%; --delay:0.8s; --dur:8s">🍃</span>
      <span class="floaty" style="--x:65%; --delay:3s;   --dur:7s">🌷</span>
      <span class="floaty" style="--x:80%; --delay:1.8s; --dur:10s">🌸</span>
      <span class="floaty" style="--x:92%; --delay:0.4s; --dur:8s">✨</span>
    </div>

    <!-- ===================== LOCKED STATE ===================== -->
    <Transition name="fade">
      <div v-if="!isUnlocked" class="lock-screen">
        <div class="lock-card" :class="{ 'lock-card--angry': wrongAttempts >= 3 }">

          <!-- Attempt counter badge -->
          <div v-if="wrongAttempts > 0" class="attempt-badge">
            {{ wrongAttempts }} galat attempt{{ wrongAttempts > 1 ? 's' : '' }} 💀
          </div>

          <div class="lock-card__badge">🌸 VIP Only • Baher Jaa 🌸</div>
          <h1 class="lock-card__title">Ankita Fan Club</h1>

          <div class="lock-card__meme-strip">
            <span>CEO of Nagpur 👑</span>
            <span>•</span>
            <span>Dil ki bht hi saaf Ladki ✨</span>
            <span>•</span>
            <span>Sab se Bhari 🔥</span>
          </div>

          <p class="lock-card__subtitle">
            <span v-if="wrongAttempts === 0">
              Ek baar prove karo ki tum sach mein fan ho,<br>
              tabhi andar aane milega, bestie. Aai shapath! 🙏
            </span>
            <span v-else-if="wrongAttempts < 3">
              Arre yaar, itna bhi nahi pata? 😤<br>
              Thoda dimag lagao, "fan" ho ya "bakwaas" ho?
            </span>
            <span v-else>
              KAY RE?! 😡 {{ wrongAttempts }} baar galat?!<br>
              Ankita ko pata chal gaya. Woh naraaz hai. Bhag.
            </span>
          </p>

          <p class="lock-card__question">
            <template v-if="wrongAttempts < 5">
              Ankita chi password kaay aahe? 🤔
            </template>
            <template v-else>
              Aata tari sahi saang! Kiti tries karnar aahe?! 😭
            </template>
          </p>

          <div class="lock-card__input-row">
            <input
              v-model="passwordInput"
              type="text"
              :placeholder="inputPlaceholder"
              class="lock-card__input"
              @keyup.enter="checkPassword"
            />
            <button class="btn btn--primary" @click="checkPassword">
              {{ wrongAttempts >= 3 ? 'Phir Try 😤' : 'Enter ✨' }}
            </button>
          </div>

          <Transition name="shake">
            <div v-if="errorMsg" class="lock-card__error" :key="errorKey">
              <span class="lock-card__error-emoji">{{ errorEmoji }}</span>
              {{ errorMsg }}
            </div>
          </Transition>

          <p class="lock-card__hint">
            <template v-if="wrongAttempts < 2">💡 Hint: Jo sach hai woh likho. Simple.</template>
            <template v-else-if="wrongAttempts < 4">💡 Bigger hint: "ankita is the ____"</template>
            <template v-else>💡 HINT: "ankita is the best" — likho re baba LIKHO 😭</template>
          </p>
        </div>
      </div>
    </Transition>

    <!-- ===================== UNLOCKED STATE ===================== -->
    <Transition name="fade">
      <div v-if="isUnlocked" class="dashboard">

        <!-- HEADER -->
        <header class="site-header">
          <div class="site-header__inner">
            <span class="site-header__crown">👑</span>
            <div>
              <h1 class="site-header__title">Welcome to the Ankita Fan Club.</h1>
              <p class="site-header__tagline">
                Nagpur chi Sabse Sundar Mulgi &mdash; officially celebrated 🌸
              </p>
              <p class="site-header__tagline2">
                United States of Nagpur ki Queen. Kiti sundar aahe! ✨
              </p>
            </div>
            <span class="site-header__crown">👑</span>
          </div>
        </header>

        <!-- SCROLLING MEME TICKER -->
        <div class="ticker-wrap">
          <div class="ticker">
            <span class="ticker__item">👑 CEO of Being Perfect</span>
            <span class="ticker__item">🔥 Rizz Level: Unmeasurable</span>
            <span class="ticker__item">✨ Ek Number Mulgi</span>
            <span class="ticker__item">🌸 Nagpur chi Shaan</span>
            <span class="ticker__item">💅 Main Character, No Debate</span>
            <span class="ticker__item">🏆 Sab se Bhari, Sab se Sundar</span>
            <span class="ticker__item">😌 Khup Chan Aahe</span>
            <span class="ticker__item">🌷 Log Dekhte Reh Jaate Hain</span>
            <span class="ticker__item">🍵 Her Smile &gt; Chai in the Morning</span>
            <span class="ticker__item">👑 CEO of Being Perfect</span>
            <span class="ticker__item">🔥 Rizz Level: Unmeasurable</span>
            <span class="ticker__item">✨ Ek Number Mulgi</span>
            <span class="ticker__item">🌸 Nagpur chi Shaan</span>
            <span class="ticker__item">💅 Main Character, No Debate</span>
            <span class="ticker__item">🏆 Sab se Bhari, Sab se Sundar</span>
            <span class="ticker__item">😌 Khup Chan Aahe</span>
          </div>
        </div>

        <!-- TODAY'S VIBE CARD -->
        <section class="section section--vibe-of-day">
          <div class="vibe-of-day">
            <div class="vibe-of-day__left">
              <p class="vibe-of-day__label">Today's Ankita Energy</p>
              <p class="vibe-of-day__text">{{ todayVibe.text }}</p>
              <p class="vibe-of-day__marathi">{{ todayVibe.marathi }}</p>
            </div>
            <div class="vibe-of-day__emoji">{{ todayVibe.emoji }}</div>
          </div>
        </section>

        <!-- GALLERY -->
        <section class="section">
          <h2 class="section__heading">📸 Ek Number Gallery</h2>
          <p class="section__sub">Proof nahi chahiye, phir bhi dekh ke khush ho jao 🙏</p>
          <div class="gallery">
            <div class="polaroid">
              <div class="polaroid__img-wrap">
                <img src="./assets/ankita-1.jpg" alt="Ankita 1" class="polaroid__img" @error="hideImg" />
                <div class="polaroid__placeholder">🌸</div>
              </div>
              <p class="polaroid__caption">Bhari aahe! Khup chan! ✨</p>
            </div>
            <div class="polaroid">
              <div class="polaroid__img-wrap">
                <img src="./assets/ankita-2.jpg" alt="Ankita 2" class="polaroid__img" @error="hideImg" />
                <div class="polaroid__placeholder">💐</div>
              </div>
              <p class="polaroid__caption">Kiti sundar aahe yaar 🌷</p>
            </div>
          </div>
          <p class="gallery__note">📁 Drop ankita-1.jpg &amp; ankita-2.jpg inside <code>src/assets/</code> to replace the flowers lol</p>
        </section>

        <!-- MEME QUOTES SECTION -->
        <section class="section">
          <h2 class="section__heading">🤣 The Meme Board</h2>
          <p class="section__sub">Certified facts. No cap. Aai shapath.</p>
          <div class="meme-grid">
            <div class="meme-card meme-card--pink">
              <p class="meme-card__quote">"Ankita ko dekhke log bhool jaate hain ki woh kya bol rahe the."</p>
              <p class="meme-card__attr">— Every single person, Nagpur 2024</p>
            </div>
            <div class="meme-card meme-card--green">
              <p class="meme-card__quote">"Kay re baba, itki sundar asli tari kasa?"</p>
              <p class="meme-card__attr">— Nagpur aunties, visibly shaken</p>
            </div>
            <div class="meme-card meme-card--cream">
              <p class="meme-card__quote">"She woke up like this. And 'this' is absolutely unreal."</p>
              <p class="meme-card__attr">— Scientists (probably)</p>
            </div>
            <div class="meme-card meme-card--pink">
              <p class="meme-card__quote">"Mala nahi mahit Ankita kaay karte, pan te perfect aahe."</p>
              <p class="meme-card__attr">— Random fan, crying</p>
            </div>
            <div class="meme-card meme-card--green">
              <p class="meme-card__quote">"Rizz nahi kehte isse. Isko 'Ankita effect' kehte hain."</p>
              <p class="meme-card__attr">— Oxford Dictionary (pending approval)</p>
            </div>
            <div class="meme-card meme-card--cream">
              <p class="meme-card__quote">"Bhayanak sundar aahe — aani tila mahit pan aahe. That's the scary part."</p>
              <p class="meme-card__attr">— This very website, just now</p>
            </div>
          </div>
        </section>

        <!-- FUN FACTS + CLUB PERKS -->
        <section class="section">
          <h2 class="section__heading">🌿 The Classified Dossier</h2>
          <p class="section__sub">Top secret info. Ankita approved nahi keli, but yahan hai anyway.</p>
          <div class="info-grid">
            <div class="info-card">
              <h3 class="info-card__title">✨ Certified Fun Facts</h3>
              <ul class="info-card__list">
                <li>Nagpur chi officially #1 sundar mulgi. Court case pending nahi, already won. ⚖️</li>
                <li>Tichi smile dekh ke din banta hai. Chai bhi nahi lagti. ☕</li>
                <li>PhD in "Being Right" — thesis defend karti hai daily basis pe. 🎓</li>
                <li>Model ho sakti thi, pan baaki models ki feelings bachane ke liye nahi hui. 💅</li>
                <li>Main character energy — 24x7, Sundays bhi, no sick leaves. 🌟</li>
                <li>"Aai shapath khup chan aahe" — literally everyone who has ever seen her. 🙏</li>
                <li>Ek number aahe, no cap, no filter, no edit. Pure factory settings. 🏭</li>
              </ul>
            </div>
            <div class="info-card">
              <h3 class="info-card__title">🎀 VIP Club Perks</h3>
              <ul class="info-card__list">
                <li>Lifetime bragging rights: "Mala Ankita personally olakhte." 💎</li>
                <li>"She was right again" support group mein priority entry. 🛋️</li>
                <li>Free emotional damage from how perfect she is — priceless. 💸</li>
                <li>Official permission to say "Mazya maitrinaichi Ankita" out loud. 📣</li>
                <li>Loyalty certificate (laminated, framed, aur valid for life). 📜</li>
                <li>Fan Club WhatsApp group mein direct add — no waiting. 📱</li>
                <li>The honor of knowing: tum sahi jagah aahe. Khup chan! ✅</li>
              </ul>
            </div>
          </div>
        </section>

        <!-- COMPLIMENT GENERATOR -->
        <section class="section">
          <h2 class="section__heading">💌 Random Compliment Generator</h2>
          <p class="section__sub">Press karo, feel karo, repeat karo. It's free. 🎁</p>
          <div class="compliment-box">
            <Transition name="compliment-swap" mode="out-in">
              <p class="compliment-box__text" :key="complimentIdx">
                <span class="compliment-box__open-quote">"</span>
                {{ currentCompliment }}
                <span class="compliment-box__close-quote">"</span>
              </p>
            </Transition>
            <button class="btn btn--compliment" @click="newCompliment">
              Ek aur do! 💐
            </button>
          </div>
        </section>

        <!-- VIBE CHECK -->
        <section class="section">
          <h2 class="section__heading">🌊 Vibe Bhejo, Fan Bano</h2>
          <p class="section__sub">Kitne vibes bhejna chahte ho aaj? Khoob bhejo, free hai! 🆓</p>
          <div class="vibe-box">
            <Transition name="compliment-swap" mode="out-in">
              <p class="vibe-box__message" :key="vibeMessage">{{ vibeMessage }}</p>
            </Transition>
            <div class="vibe-box__counter-wrap">
              <span class="vibe-box__count">{{ vibeCount }}</span>
              <span class="vibe-box__count-label">vibes sent</span>
            </div>
            <button class="btn btn--vibe" @click="sendVibe">
              💚 Vibe Bhejo! 💚
            </button>
            <div v-if="vibeCount > 0" class="vibe-box__bar-wrap">
              <div class="vibe-box__bar" :style="{ width: vibeBarWidth }"></div>
            </div>
          </div>
        </section>

        <!-- LOYALTY OATH -->
        <section class="section">
          <h2 class="section__heading">🤝 Fan Club Loyalty Oath</h2>
          <p class="section__sub">Ek baar bolna padega. Dil se. Aai shapath.</p>
          <div class="oath-box">
            <div class="oath-scroll">
              <p class="oath-scroll__text">
                "Main, {{ oathName || '(apna naam likho)' }}, aaj is paavan din pe yeh pratigya leta/leti hoon ki<br>
                Ankita sabse sundar hai, sabse bhari hai, aur hamesha sahi hoti hai.<br>
                Yeh sach hai. Yeh sach tha. Yeh sach rahega. Aai shapath. Ek number. Khup chan."
              </p>
            </div>
            <div class="oath-box__controls">
              <input
                v-model="oathName"
                type="text"
                class="lock-card__input"
                placeholder="Apna naam yahan likho..."
                maxlength="30"
              />
              <button class="btn btn--primary" @click="takeOath" :disabled="oathTaken">
                {{ oathTaken ? 'Oath Li Gayi! ✅' : 'Oath Lo 🙏' }}
              </button>
            </div>
            <Transition name="fade">
              <div v-if="oathTaken" class="oath-box__confirmed">
                🎉 Badhaai ho, {{ oathName }}! Tumhara naam ab Ankita Fan Club ke Hall of Fame mein hai!<br>
                <span class="oath-box__marathi">Amhi sagale fans aahe. Khup chan! 🌸</span>
              </div>
            </Transition>
          </div>
        </section>

        <!-- PRANK -->
        <section class="section">
          <h2 class="section__heading">🫣 The Great Nagpur Debate</h2>
          <p class="section__sub">Do buttons, ek sahi jawab. Dusra button toh sirf dikhawa hai. 😈</p>
          <div class="prank-box">
            <p class="prank-box__question">Kay re — Ankita always right aaste ka? 🧐</p>
            <p class="prank-box__subtext">
              (Sahi jawab "Haan" aahe. Doosra option exist nahi karta technically.)
            </p>
            <div class="prank-box__buttons">
              <button class="btn btn--yes" @click="onYesClick">
                Haan, nakki! 🥰
              </button>
              <button
                class="btn btn--no"
                :style="noBtnStyle"
                @mouseover="runAway"
                @touchstart.prevent="runAway"
                @click="onNoClick"
              >
                {{ noButtonLabel }}
              </button>
            </div>
            <Transition name="fade">
              <div v-if="prankMsg" class="prank-box__result">
                <p>{{ prankMsg }}</p>
              </div>
            </Transition>
          </div>
        </section>

        <!-- FOOTER -->
        <footer class="site-footer">
          <div class="site-footer__hearts">
            <span v-for="i in 5" :key="i">🌸</span>
          </div>
          <p>Made with 💚 &amp; 🌷 for the undisputed Queen of Nagpur.</p>
          <p class="site-footer__marathi">Ankita bhari aahe. Ankita ek number aahe. Iti siddham. 🙏</p>
          <p class="site-footer__sub">© {{ currentYear }} Ankita Fan Club — Unauthorized entry toh already impossible tha. Aai shapath.</p>
        </footer>

      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// ── LOCK ──────────────────────────────────────────────────────────────────────
const isUnlocked   = ref(false)
const passwordInput = ref('')
const errorMsg     = ref('')
const errorEmoji   = ref('')
const errorKey     = ref(0)
const wrongAttempts = ref(0)
const CORRECT_PASSWORD = 'ankita is the best'

const inputPlaceholders = [
  'Soch ke likhna...',
  'Hint dekha? Phir bhi nahi aya?',
  'Ek baar aur try karo re...',
  'Bhai seriously likh do ab...',
  'LIKHO. ABHI. PLEASE. 😭',
]
const inputPlaceholder = computed(() =>
  inputPlaceholders[Math.min(wrongAttempts.value, inputPlaceholders.length - 1)]
)

const funnyErrors = [
  { msg: 'Galat! Teri friendship card cancel hone wali hai yaar.',       emoji: '😤' },
  { msg: 'Nahi nahi nahi. Hint dekha? Phir bhi galat? Kay re baba!',    emoji: '🤦' },
  { msg: 'Bhai seriously? Itna bhi nahi pata? Chi chwi! Embarrassing.', emoji: '😂' },
  { msg: 'Security breach attempt. Ankita ko personally report kiya.', emoji: '📢' },
  { msg: 'Wrong. Fan club se officially ban. Baher jaa!',               emoji: '🚫' },
  { msg: 'Aai shapath yaar — try again, better energy ke saath please.', emoji: '🙄' },
  { msg: 'Beta yeh toh ekdum easy tha. Phir bhi galat? Haaye haaye!',   emoji: '😩' },
  { msg: 'Mala nahi mahit tula kasa saangaychay... ANKITA IS THE BEST!', emoji: '🥲' },
  { msg: 'Log chand pe gaye, tum password nahi dekh sakte? Bakwas nako!', emoji: '🌝' },
  { msg: 'Ek number galat jawab. Kiti baar deta rahega yaar?!',          emoji: '💀' },
]

function checkPassword() {
  if (passwordInput.value.trim().toLowerCase() === CORRECT_PASSWORD) {
    errorMsg.value = ''
    isUnlocked.value = true
  } else {
    wrongAttempts.value++
    const idx = Math.floor(Math.random() * funnyErrors.length)
    errorMsg.value  = funnyErrors[idx].msg
    errorEmoji.value = funnyErrors[idx].emoji
    errorKey.value++
    passwordInput.value = ''
  }
}

function hideImg(e) {
  e.target.style.display = 'none'
}

// ── TODAY'S VIBE ──────────────────────────────────────────────────────────────
const vibes = [
  { emoji: '👑', text: 'Main character, no competition.',        marathi: 'Aaj tichi vibe ekdum top level aahe.' },
  { emoji: '🌸', text: 'Blooming and absolutely unbothered.',    marathi: 'Khup chan distey aaj. Ek number!' },
  { emoji: '✨', text: 'Slay mode: permanently on.',            marathi: 'Bhayanak level chi sundar mulgi.' },
  { emoji: '💅', text: 'Unbothered. Moisturised. Thriving.',    marathi: 'Kiti cool aahe — amhala nahi hote kadhi.' },
  { emoji: '🔥', text: 'Too hot to handle, Nagpur walo.',       marathi: 'Nakki aaj Ankita chi energy bhari aahe.' },
  { emoji: '🌿', text: 'Calm, collected, completely unreal.',   marathi: 'Shant, sundar, aani ek number.' },
  { emoji: '💐', text: 'Serving looks like it is her job.',     marathi: 'Tichi style bagh — maja aayi!' },
]
const todayVibe = vibes[new Date().getDay() % vibes.length]

// ── COMPLIMENT GENERATOR ──────────────────────────────────────────────────────
const compliments = [
  'Ankita itni sundar hai ki filter companies usse royalties deti hain.',
  'Tichi smile dekh ke Nagpur ki bijli 10% zyada bright hoti hai.',
  'Khup sundar aahe yaar — shabd nahi hain, bas "wow" aahe.',
  'She walked in and the whole room forgot what they were saying. Typical Ankita.',
  'Ek number mulgi, ek number vibes — Nagpur ka sabse bada export yahi hai.',
  'Log aainate khud ko dekhte hain, Ankita aainate duniya ko dekhti hai. Differently. Better.',
  'Bhari aahe, chan aahe, aani khup jast cute aahe. Iti siddham.',
  'Scientists still can\'t explain the Ankita Effect. They\'ve given up trying.',
  'Her presence walks into a room before she does. That\'s just how it works.',
  'Rizz nahi hai — yeh toh kuch aur hi aahe. "Ankita" kehte hain isse.',
  'Log kehte hain beauty is subjective. Phir Ankita ko dekhte hain. Phir chup ho jaate hain.',
  'Aai shapath — itki sundar asli tari kasa? Physics ko nahi pata.',
  'Khup chan distey nustich nahi — she is genuinely, objectively, measurably perfect.',
  'Nagpur chi shaan, India chi jaan, fan club chi mahan — bas Ankita hi aahe.',
  'Even her "I\'m tired" looks better than everyone else\'s "I got 8 hours of sleep."',
]
const complimentIdx = ref(0)
const currentCompliment = computed(() => compliments[complimentIdx.value])

function newCompliment() {
  let next
  do { next = Math.floor(Math.random() * compliments.length) }
  while (next === complimentIdx.value)
  complimentIdx.value = next
}

// ── VIBE CHECK ────────────────────────────────────────────────────────────────
const vibeCount = ref(0)

const vibeMessage = computed(() => {
  const c = vibeCount.value
  if (c === 0)   return '✨ Press karo aur usse vibes bhejo!'
  if (c < 5)     return 'Thodi si vibes gayi. Theek hai, start toh hua. 🌱'
  if (c < 10)    return 'Arre wah! Vibes aane lagi! Aur bhejo! 🌿'
  if (c < 20)    return 'Maja aayi! Vibes strong ho rahi hain. 💚'
  if (c < 35)    return 'Full power mode ON! Ek number chal raha hai! 🌊'
  if (c < 50)    return 'Aai shapath! Vibe overload! She can feel it!! 🔥'
  if (c < 75)    return 'BHAYANAK! MAXIMUM VIBE CAPACITY!! 🚀'
  if (c < 100)   return 'Nakki yaar, ruk ja thoda... universe overwhelmed ho gaya. 😭'
  return           'VIBE UNIVERSE MEIN OVERFLOW. Ankita ne notice kiya. Tujhe stars milenge. 💫'
})

const vibeBarWidth = computed(() => `${Math.min((vibeCount.value / 100) * 100, 100)}%`)

function sendVibe() { vibeCount.value++ }

// ── LOYALTY OATH ──────────────────────────────────────────────────────────────
const oathName  = ref('')
const oathTaken = ref(false)
function takeOath() {
  if (oathName.value.trim()) oathTaken.value = true
}

// ── PRANK ─────────────────────────────────────────────────────────────────────
const noLabels     = ['Nahi', 'Nakki Nahi', 'Nope', 'Nako!', 'Chi!', 'Nahi re...']
const noLabelIdx   = ref(0)
const noButtonLabel = computed(() => noLabels[noLabelIdx.value])

const noBtnStyle = ref({ position: 'relative', top: '0px', left: '0px' })
const prankMsg   = ref('')

function runAway() {
  const maxX = 210, maxY = 80
  const x = (Math.random() * maxX * 2 - maxX).toFixed(0)
  const y = (Math.random() * maxY * 2 - maxY).toFixed(0)
  noLabelIdx.value = (noLabelIdx.value + 1) % noLabels.length
  noBtnStyle.value = {
    position: 'relative',
    top:  `${y}px`,
    left: `${x}px`,
    transition: 'top 0.1s ease, left 0.1s ease',
  }
}

function onYesClick() {
  prankMsg.value = '🎉 Sahi jawab! Haan, nakki! Certificate of Sane Thinking diya jaata hai aapko! Khup chan! 🌸'
}
function onNoClick() {
  prankMsg.value = 'HOW?! Hacker aahes ka tu?! Ankita ki oath todi tune! Kay re baba!! 😱💀'
}

// ── MISC ──────────────────────────────────────────────────────────────────────
const currentYear = new Date().getFullYear()
</script>

<style scoped>
/* ── RESET ────────────────────────────────────────────────────── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

/* ── TOKENS ───────────────────────────────────────────────────── */
.app-wrapper {
  --sage:        #8faa8a;
  --sage-light:  #b5cbb0;
  --sage-dark:   #5f8059;
  --cream:       #fdf8f0;
  --cream-dark:  #f2ead8;
  --blush:       #f2c4c4;
  --blush-light: #fde8e8;
  --blush-dark:  #d98989;
  --text:        #4a4442;
  --text-light:  #8a7f7c;
  --shadow-sm:   0 2px 14px rgba(100,140,95,.14);
  --shadow-md:   0 6px 30px rgba(100,140,95,.20);
  --shadow-lg:   0 14px 50px rgba(100,140,95,.26);
  --r-sm: 12px; --r-md: 20px; --r-lg: 32px;

  position: relative;
  min-height: 100vh;
  overflow-x: hidden;
  background:
    radial-gradient(ellipse at 15% 15%, rgba(179,203,176,.38) 0%, transparent 52%),
    radial-gradient(ellipse at 85% 85%, rgba(242,196,196,.32) 0%, transparent 52%),
    linear-gradient(160deg, #e3ece2 0%, #fdf8f0 50%, #fde8e8 100%);
  font-family: 'Georgia', 'Times New Roman', serif;
  color: var(--text);
}

/* ── FLOATING EMOJIS ─────────────────────────────────────────── */
.floaties { position: fixed; inset: 0; pointer-events: none; z-index: 0; overflow: hidden; }
.floaty {
  position: absolute;
  bottom: -60px;
  left: var(--x);
  font-size: 1.5rem;
  opacity: 0;
  animation: floatUp var(--dur) var(--delay) infinite ease-in-out;
}
@keyframes floatUp {
  0%   { transform: translateY(0)   rotate(0deg);  opacity: 0; }
  10%  { opacity: 0.55; }
  90%  { opacity: 0.35; }
  100% { transform: translateY(-110vh) rotate(360deg); opacity: 0; }
}

/* ── TRANSITIONS ──────────────────────────────────────────────── */
.fade-enter-active, .fade-leave-active { transition: opacity .55s ease, transform .55s ease; }
.fade-enter-from  { opacity: 0; transform: translateY(18px); }
.fade-leave-to    { opacity: 0; transform: translateY(-18px); }

.shake-enter-active { animation: shake .42s ease; }
@keyframes shake {
  0%,100%{ transform:translateX(0); }
  18%    { transform:translateX(-9px); }
  36%    { transform:translateX(9px); }
  54%    { transform:translateX(-6px); }
  72%    { transform:translateX(6px); }
}

.compliment-swap-enter-active, .compliment-swap-leave-active { transition: opacity .35s ease, transform .35s ease; }
.compliment-swap-enter-from { opacity:0; transform:translateY(10px); }
.compliment-swap-leave-to   { opacity:0; transform:translateY(-10px); }

/* ── LOCK SCREEN ──────────────────────────────────────────────── */
.lock-screen {
  position: relative; z-index: 1;
  display: flex; align-items: center; justify-content: center;
  min-height: 100vh; padding: 24px;
}

.lock-card {
  background: rgba(253,248,240,.92);
  backdrop-filter: blur(18px);
  border: 1.5px solid rgba(181,203,176,.55);
  border-radius: var(--r-lg);
  padding: 52px 44px 44px;
  max-width: 500px; width: 100%;
  text-align: center;
  box-shadow: var(--shadow-lg), 0 0 0 1px rgba(255,255,255,.6) inset;
  transition: border-color .4s;
}
.lock-card--angry {
  border-color: rgba(217,137,137,.6);
  box-shadow: var(--shadow-lg), 0 0 0 1px rgba(255,200,200,.5) inset;
}

.attempt-badge {
  display: inline-block;
  background: linear-gradient(135deg, #fde8e8, #fcc);
  border: 1px solid var(--blush);
  border-radius: 999px;
  padding: 4px 16px;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 0.74rem; font-weight: 700;
  color: var(--blush-dark);
  margin-bottom: 14px;
  animation: pulse 1.5s ease infinite;
}
@keyframes pulse {
  0%,100% { transform: scale(1); }
  50%      { transform: scale(1.05); }
}

.lock-card__badge {
  display: inline-block;
  background: linear-gradient(135deg, var(--blush-light), var(--cream-dark));
  border: 1px solid var(--blush);
  border-radius: 999px;
  padding: 6px 22px;
  font-size: 0.74rem;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  letter-spacing: .10em; text-transform: uppercase;
  color: var(--blush-dark); margin-bottom: 18px;
}

.lock-card__title {
  font-size: 2.5rem; font-weight: 700;
  color: var(--sage-dark); letter-spacing: -.025em;
  line-height: 1.15; margin-bottom: 12px;
}

.lock-card__meme-strip {
  display: flex; align-items: center; justify-content: center;
  gap: 10px; flex-wrap: wrap;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 0.78rem; font-weight: 600;
  color: var(--sage-dark); margin-bottom: 18px;
  background: linear-gradient(135deg, rgba(181,203,176,.25), rgba(242,196,196,.20));
  border-radius: 999px; padding: 8px 20px;
}

.lock-card__subtitle {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 0.90rem; color: var(--text-light);
  line-height: 1.75; margin-bottom: 24px;
}

.lock-card__question {
  font-size: 1.08rem; font-style: italic;
  color: var(--text); margin-bottom: 22px;
}

.lock-card__input-row { display: flex; gap: 10px; margin-bottom: 14px; }

.lock-card__input {
  flex: 1; padding: 13px 18px;
  border: 1.5px solid var(--sage-light); border-radius: var(--r-sm);
  font-family: 'Helvetica Neue', Arial, sans-serif; font-size: .95rem;
  background: var(--cream); color: var(--text); outline: none;
  transition: border-color .2s, box-shadow .2s;
}
.lock-card__input:focus {
  border-color: var(--sage);
  box-shadow: 0 0 0 3px rgba(143,170,138,.20);
}
.lock-card__input::placeholder { color: var(--text-light); font-style: italic; }

.lock-card__error {
  display: flex; align-items: center; gap: 10px;
  font-family: 'Helvetica Neue', Arial, sans-serif; font-size: .86rem;
  color: var(--blush-dark); background: var(--blush-light);
  border: 1px solid var(--blush); border-radius: var(--r-sm);
  padding: 11px 16px; margin-bottom: 12px; line-height: 1.5; text-align: left;
}
.lock-card__error-emoji { font-size: 1.3rem; flex-shrink: 0; }

.lock-card__hint {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .78rem; color: var(--text-light);
  font-style: italic; margin-top: 16px;
}

/* ── BUTTONS ──────────────────────────────────────────────────── */
.btn {
  font-family: 'Helvetica Neue', Arial, sans-serif; font-weight: 600;
  border: none; border-radius: var(--r-sm); cursor: pointer;
  letter-spacing: .02em;
  transition: transform .18s ease, box-shadow .18s ease, background .2s ease;
}
.btn:active { transform: scale(0.96) !important; }
.btn:disabled { opacity: .6; cursor: default; }

.btn--primary {
  background: linear-gradient(135deg, var(--sage), var(--sage-dark));
  color: #fff; padding: 13px 22px; font-size: .88rem; box-shadow: var(--shadow-sm);
}
.btn--primary:not(:disabled):hover {
  background: linear-gradient(135deg, var(--sage-dark), #4d6e48);
  box-shadow: var(--shadow-md); transform: translateY(-2px);
}

.btn--vibe {
  background: linear-gradient(135deg, var(--sage-light), var(--sage));
  color: #fff; padding: 15px 38px; font-size: 1rem;
  border-radius: var(--r-md); box-shadow: var(--shadow-md);
}
.btn--vibe:hover {
  background: linear-gradient(135deg, var(--sage), var(--sage-dark));
  box-shadow: var(--shadow-lg); transform: translateY(-3px);
}

.btn--yes {
  background: linear-gradient(135deg, var(--blush-light), var(--blush));
  color: var(--blush-dark); padding: 13px 30px;
  border-radius: var(--r-md); font-size: .95rem;
  border: 1.5px solid var(--blush); box-shadow: var(--shadow-sm);
}
.btn--yes:hover {
  background: linear-gradient(135deg, var(--blush), var(--blush-dark));
  color: #fff; transform: translateY(-2px); box-shadow: var(--shadow-md);
}

.btn--no {
  background: linear-gradient(135deg, #ece8e4, #dedad6);
  color: var(--text-light); padding: 13px 30px;
  border-radius: var(--r-md); font-size: .95rem;
  border: 1.5px solid #ccc; user-select: none;
}

.btn--compliment {
  background: linear-gradient(135deg, var(--blush-light), var(--blush));
  color: var(--blush-dark); padding: 13px 30px;
  border-radius: var(--r-md); font-size: .92rem;
  border: 1.5px solid var(--blush); box-shadow: var(--shadow-sm);
}
.btn--compliment:hover {
  background: linear-gradient(135deg, var(--blush), var(--blush-dark));
  color: #fff; transform: translateY(-2px); box-shadow: var(--shadow-md);
}

/* ── DASHBOARD ────────────────────────────────────────────────── */
.dashboard {
  position: relative; z-index: 1;
  max-width: 1000px; margin: 0 auto;
  padding: 0 24px 72px;
}

/* ── HEADER ───────────────────────────────────────────────────── */
.site-header { text-align: center; padding: 64px 0 32px; }
.site-header__inner {
  display: flex; align-items: center; justify-content: center; gap: 22px;
}
.site-header__crown { font-size: 2.4rem; animation: sway 3s ease-in-out infinite; }
@keyframes sway {
  0%,100% { transform: rotate(-8deg); }
  50%      { transform: rotate(8deg); }
}
.site-header__title {
  font-size: clamp(1.85rem, 4vw, 2.8rem); font-weight: 700;
  color: var(--sage-dark); letter-spacing: -.03em;
  line-height: 1.15; margin-bottom: 8px;
}
.site-header__tagline {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .93rem; color: var(--text-light); font-style: italic;
}
.site-header__tagline2 {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .82rem; color: var(--blush-dark); margin-top: 4px;
  font-style: italic;
}

/* ── TICKER ───────────────────────────────────────────────────── */
.ticker-wrap {
  overflow: hidden;
  background: linear-gradient(135deg, rgba(181,203,176,.28), rgba(242,196,196,.22));
  border: 1px solid rgba(181,203,176,.35);
  border-radius: 999px;
  padding: 10px 0;
  margin-bottom: 28px;
  white-space: nowrap;
}
.ticker {
  display: inline-flex;
  animation: ticker 28s linear infinite;
}
.ticker__item {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .82rem; font-weight: 600;
  color: var(--sage-dark); padding: 0 32px;
  letter-spacing: .04em;
}
@keyframes ticker {
  0%   { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}

/* ── TODAY'S VIBE ─────────────────────────────────────────────── */
.section--vibe-of-day { padding: 28px 36px; }
.vibe-of-day {
  display: flex; align-items: center; justify-content: space-between; gap: 20px;
}
.vibe-of-day__label {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .75rem; font-weight: 700; letter-spacing: .10em;
  text-transform: uppercase; color: var(--sage); margin-bottom: 8px;
}
.vibe-of-day__text {
  font-size: 1.2rem; color: var(--sage-dark); font-style: italic; margin-bottom: 6px;
}
.vibe-of-day__marathi {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .84rem; color: var(--text-light);
}
.vibe-of-day__emoji { font-size: 3.5rem; flex-shrink: 0; }

/* ── SECTIONS ─────────────────────────────────────────────────── */
.section {
  background: rgba(253,248,240,.80);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(181,203,176,.38);
  border-radius: var(--r-lg); padding: 44px 40px;
  margin-bottom: 28px;
  box-shadow: var(--shadow-sm), 0 0 0 1px rgba(255,255,255,.5) inset;
}
.section__heading {
  font-size: 1.5rem; color: var(--sage-dark);
  margin-bottom: 5px; letter-spacing: -.01em;
}
.section__sub {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .87rem; color: var(--text-light);
  font-style: italic; margin-bottom: 28px;
}

/* ── GALLERY ──────────────────────────────────────────────────── */
.gallery { display: flex; gap: 36px; justify-content: center; flex-wrap: wrap; }

.polaroid {
  background: #fff; padding: 14px 14px 44px; border-radius: 3px;
  box-shadow: var(--shadow-md), 2px 2px 0 rgba(0,0,0,.04);
  max-width: 270px; width: 100%;
  transition: transform .3s ease, box-shadow .3s ease;
  transform: rotate(-1.8deg);
}
.polaroid:nth-child(2) { transform: rotate(2deg); }
.polaroid:hover { transform: rotate(0deg) scale(1.04) !important; box-shadow: var(--shadow-lg); }

.polaroid__img-wrap {
  width: 100%; aspect-ratio: 4/5;
  background: linear-gradient(135deg, var(--cream-dark), var(--blush-light));
  border-radius: 2px; overflow: hidden; position: relative;
  display: flex; align-items: center; justify-content: center;
}
.polaroid__img { width:100%; height:100%; object-fit:cover; display:block; position:relative; z-index:1; }
.polaroid__placeholder {
  font-size: 4.5rem; position: absolute; inset: 0;
  display: flex; align-items: center; justify-content: center;
  opacity: .45; z-index: 0;
}
.polaroid__caption {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .80rem; color: var(--text-light);
  text-align: center; margin-top: 16px; font-style: italic;
}
.gallery__note {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .76rem; color: var(--text-light); font-style: italic;
  text-align: center; margin-top: 24px;
}
.gallery__note code {
  background: var(--cream-dark); padding: 2px 6px;
  border-radius: 4px; font-family: monospace; font-style: normal;
  font-size: .80rem;
}

/* ── MEME GRID ────────────────────────────────────────────────── */
.meme-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 18px;
}
.meme-card {
  border-radius: var(--r-md); padding: 22px 20px;
  border: 1px solid rgba(181,203,176,.30);
  box-shadow: var(--shadow-sm);
  transition: transform .25s ease, box-shadow .25s ease;
}
.meme-card:hover { transform: translateY(-4px); box-shadow: var(--shadow-md); }
.meme-card--pink  { background: linear-gradient(135deg, #fde8e8, #fdf0f5); }
.meme-card--green { background: linear-gradient(135deg, #e8f0e6, #edf5ea); }
.meme-card--cream { background: linear-gradient(135deg, var(--cream), var(--cream-dark)); }
.meme-card__quote {
  font-size: .90rem; font-style: italic;
  color: var(--text); line-height: 1.65; margin-bottom: 12px;
}
.meme-card__attr {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .74rem; color: var(--text-light);
  font-style: normal; letter-spacing: .02em;
}

/* ── INFO GRID ────────────────────────────────────────────────── */
.info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 22px; }
.info-card {
  background: linear-gradient(150deg, var(--cream), var(--cream-dark));
  border: 1px solid rgba(181,203,176,.42);
  border-radius: var(--r-md); padding: 28px 24px; box-shadow: var(--shadow-sm);
}
.info-card__title {
  font-size: 1.06rem; color: var(--sage-dark); margin-bottom: 16px;
  padding-bottom: 12px; border-bottom: 1.5px dashed var(--sage-light);
}
.info-card__list { list-style: none; display: flex; flex-direction: column; gap: 13px; }
.info-card__list li {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .875rem; color: var(--text);
  line-height: 1.65; padding-left: 20px; position: relative;
}
.info-card__list li::before {
  content: '✦'; position: absolute; left: 0;
  color: var(--sage); font-size: .68rem; top: 4px;
}

/* ── COMPLIMENT BOX ───────────────────────────────────────────── */
.compliment-box {
  display: flex; flex-direction: column; align-items: center; gap: 24px;
}
.compliment-box__text {
  font-size: 1.15rem; font-style: italic;
  color: var(--sage-dark); text-align: center; line-height: 1.7;
  max-width: 600px;
  background: linear-gradient(135deg, var(--cream), var(--blush-light));
  border: 1px solid rgba(242,196,196,.45);
  border-radius: var(--r-md); padding: 26px 32px;
  box-shadow: var(--shadow-sm);
}
.compliment-box__open-quote,
.compliment-box__close-quote {
  color: var(--blush-dark); font-size: 1.6rem;
  font-style: normal; line-height: 0; vertical-align: -0.3em;
}

/* ── VIBE BOX ─────────────────────────────────────────────────── */
.vibe-box { display: flex; flex-direction: column; align-items: center; gap: 22px; }
.vibe-box__message {
  font-size: 1.15rem; font-style: italic; color: var(--sage-dark);
  text-align: center; min-height: 1.8em;
}
.vibe-box__counter-wrap { display: flex; align-items: baseline; gap: 9px; }
.vibe-box__count { font-size: 3.4rem; font-weight: 700; color: var(--sage-dark); line-height: 1; }
.vibe-box__count-label {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .78rem; color: var(--text-light);
  letter-spacing: .08em; text-transform: uppercase;
}
.vibe-box__bar-wrap {
  width: 100%; max-width: 400px; height: 8px;
  background: var(--cream-dark); border-radius: 999px; overflow: hidden;
}
.vibe-box__bar {
  height: 100%;
  background: linear-gradient(90deg, var(--sage-light), var(--sage-dark));
  border-radius: 999px; transition: width .45s ease;
}

/* ── LOYALTY OATH ─────────────────────────────────────────────── */
.oath-box { display: flex; flex-direction: column; gap: 22px; }
.oath-scroll {
  background: linear-gradient(135deg, #fef9f0, #fdf5e4);
  border: 1.5px dashed var(--sage-light); border-radius: var(--r-md);
  padding: 28px 32px;
}
.oath-scroll__text {
  font-size: 1rem; font-style: italic; color: var(--text);
  line-height: 1.85; text-align: center;
}
.oath-box__controls { display: flex; gap: 12px; }
.oath-box__confirmed {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .92rem; color: var(--sage-dark);
  background: linear-gradient(135deg, #e8f0e6, #edf5ea);
  border: 1.5px solid var(--sage-light); border-radius: var(--r-md);
  padding: 18px 24px; text-align: center; line-height: 1.7;
  box-shadow: var(--shadow-sm);
}
.oath-box__marathi { font-size: 1rem; color: var(--blush-dark); font-style: italic; }

/* ── PRANK BOX ────────────────────────────────────────────────── */
.prank-box { display: flex; flex-direction: column; align-items: center; gap: 20px; }
.prank-box__question { font-size: 1.22rem; font-style: italic; color: var(--text); }
.prank-box__subtext {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .80rem; color: var(--text-light); font-style: italic;
}
.prank-box__buttons {
  display: flex; gap: 24px; align-items: center;
  min-height: 140px; width: 100%; max-width: 520px;
  justify-content: center; position: relative; overflow: visible;
}
.prank-box__result {
  font-family: 'Helvetica Neue', Arial, sans-serif; font-size: .98rem;
  color: var(--sage-dark);
  background: linear-gradient(135deg, var(--cream), #e6ede5);
  border: 1.5px solid var(--sage-light); border-radius: var(--r-md);
  padding: 16px 30px; text-align: center; box-shadow: var(--shadow-sm);
}

/* ── FOOTER ───────────────────────────────────────────────────── */
.site-footer { text-align: center; padding: 44px 0 24px; }
.site-footer__hearts { font-size: 1.2rem; letter-spacing: 8px; margin-bottom: 14px; }
.site-footer p {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: .88rem; color: var(--text-light); line-height: 2;
}
.site-footer__marathi { font-style: italic; color: var(--sage-dark) !important; }
.site-footer__sub { font-size: .76rem !important; opacity: .65; font-style: italic; }

/* ── RESPONSIVE ───────────────────────────────────────────────── */
@media (max-width: 768px) {
  .meme-grid { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 600px) {
  .section { padding: 28px 18px; }
  .info-grid, .meme-grid { grid-template-columns: 1fr; }
  .lock-card { padding: 36px 20px 30px; }
  .lock-card__input-row { flex-direction: column; }
  .lock-card__title { font-size: 2rem; }
  .lock-card__meme-strip { gap: 6px; font-size: .70rem; }
  .site-header__inner { flex-direction: column; gap: 10px; }
  .prank-box__buttons { min-height: 170px; }
  .gallery { flex-direction: column; align-items: center; }
  .vibe-of-day { flex-direction: column; text-align: center; }
  .oath-box__controls { flex-direction: column; }
  .compliment-box__text { padding: 20px; font-size: 1rem; }
}
</style>
