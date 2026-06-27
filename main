/**
 * 👑 Copyright: Sulaiman Abdullrhman Alnathir
 * 📸 Instagram: s_k518
 */

const { Client } = require("discord.js-selfbot-v13");

// [AR] مصفوفة التوكنات - ضع التوكنات هنا
// [EN] Array of tokens - Insert your tokens here
const tokens = [
  // "there but the token ",
  // "اضف التوكن هنا ",
];

// [AR] إعدادات الخادم والروم الصوتي
// [EN] Guild and Voice channel configuration
const GUILD_ID = "ايدي السيرفر |server id ";
const VOICE_CHANNEL_ID = "ايدي الروم | room id ";

// [AR] دالة الاتصال والمحافظة على التواجد داخل الروم الصوتي
// [EN] Function to connect and maintain connection in the voice channel
const stayInVoice = (client) => {
  try {
    client.ws.shards.forEach(shard => shard.send({
      op: 4,
      d: {
        guild_id: GUILD_ID,
        channel_id: VOICE_CHANNEL_ID,
        self_mute: false,
        self_deaf: false
      }
    }));
  } catch (e) {}
};

// [AR] تشغيل الحسابات بشكل تتابعي
// [EN] Initializing and logging in the accounts sequentially
tokens.forEach((token, index) => {
  if (!token) return;
  const client = new Client({ checkUpdate: false });

  // [AR] حدث تشغيل الحساب بنجاح
  // [EN] Event triggered when the account is ready
  client.on("ready", () => {
    console.log(`✅ [${index + 1}] Online: ${client.user.tag}`);
    stayInVoice(client);
  });

  // [AR] مراقبة حالة الصوت وإعادة الدخول التلقائي في حال الخروج أو الكتم
  // [EN] Monitor voice state and auto-reconnect if disconnected or muted
  client.on("voiceStateUpdate", (oldState, newState) => {
    if (newState.id === client.user.id) {
      if (newState.channelId !== VOICE_CHANNEL_ID || newState.selfMute || newState.selfDeaf) {
        setTimeout(() => stayInVoice(client), 5000);
      }
    }
  });

  // [AR] تسجيل دخول تدريجي (حساب كل ثانية) لتفادي حظر الشبكة أو الضغط العالي
  // [EN] Staggered login (1s delay per token) to prevent rate limits and heavy load
  setTimeout(() => {
    client.login(token).catch(err => console.error(`❌ Token ${index + 1} Error: ${err.message}`));
  }, index * 1000);
});
