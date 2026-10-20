# Rollcard

**Your camera roll, turned into a personality card.**

Rollcard looks through the photos on your phone, works out the themes that say the most about you, and turns them into a card you can share.

<p align="center">
  <img src="docs/rollcard-demo.png" alt="A Rollcard showing a gallery's top six themes, including Battlestation, Summit Chaser and Loud Pipes Save Lives, each with a rarity and a matching photo" width="320">
</p>

## What it does

- **Finds your themes.** Rollcard analyses your camera roll with an AI vision-language model and matches it against a library of 180+ personas, such as *Battlestation*, *Summit Chaser*, *Float Like a Butterfly* and *Loud Pipes Save Lives*.
- **Shows the evidence.** Each theme on the card is paired with one of your own photos that matched it.
- **Adds rarity.** Every theme gets a rarity, such as Common or Rare, and so does the card as a whole.
- **Lets you share it.** One tap to share your Rollcard.

## How it works

1. **Read the gallery.** With the user's permission, the app loads photos from the camera roll.
2. **Match photos to personas.** Each persona has a set of text descriptions of what its photos look like. [SigLIP](https://arxiv.org/abs/2303.15343), a vision-language model, scores how well each photo matches each description. Because SigLIP compares images with text directly, adding a new persona only needs new descriptions, not new training data.
3. **Apply thresholds.** A photo counts towards a persona only if its score clears that persona's confidence threshold. One photo can count towards several personas, so this is multi-label classification.
4. **Filter out false matches.** A photo can match a description without telling you anything about the person, so two rules handle that:
   - **Utility photos don't count.** Screenshots, receipts and notes are excluded from scoring.
   - **Personas have a signal type.**
     - *Embodiment* personas need the photo to show the user actually doing the thing. A saved picture of someone else's tattoo shouldn't make you "the tattooed one".
     - *Affinity* personas accept saved images, because saving anime art still says you're into anime.
5. **Build the card.** The top themes go onto the card, each shown with a photo that matched it.

## Persona library

Each persona is defined in JSON with:

- a display name
- a tagline
- a set of detection prompts
- a confidence threshold
- a signal type (embodiment or affinity)

This keeps the library easy to grow and tune without touching the app code.

## Tech stack

| Part | Technology |
|---|---|
| Mobile app | React Native, Expo |
| Image–text matching | SigLIP |
| Backend | Firebase |
| Image hosting | Cloudinary |

## Running it locally

You'll need Node.js, the Expo Go app on your phone, and your own Firebase and Cloudinary accounts, with their config added to the app.

```bash
git clone https://github.com/Rokuu010/rollcard.git
cd rollcard
npm install
npx expo start
```

Scan the QR code with Expo Go. Your phone and computer need to be on the same Wi-Fi network; if they can't be, run `npx expo start --tunnel` instead.

## Status

Complete: working end to end, from gallery scan to shareable card.

## Authors

Built by [Rohan](https://github.com/Rokuu010) and [Ken](https://github.com/friend-username).
