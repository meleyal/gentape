---
title: Phasing
color: "#b263f9"
author: meleyal
author_url: https://meleyal.com/
---

<script>
const { ring, metronome, sampler, samples } = gen;

(async () => {
  const context = new AudioContext();
  const piano1 = await sampler(context, {
    samples: samples.piano,
    output: context.destination
  });
  const piano2 = await sampler(context, {
    samples: samples.piano,
    output: context.destination
  });
  const metro = metronome(context, 14);

  const notes = ring([
    "E4",
    "F#4",
    "B4",
    "C#5",
    "D5",
    "F#4",
    "E4",
    "C#5",
    "B4",
    "F#4",
    "D5",
    "C#5"
  ]);

  let phase = 0;

  metro.on("tick", tick => {
    const note = notes(tick);
    piano1(note, { start: now });
    piano2(note, { start: now + phase });
    phase += 0.003;
  });
  metro.start();
})();
</script>
