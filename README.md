# YAGPDB CubeCobra P1P1 Command

A custom **YAGPDB** command that generates a **random CubeCobra sample pack URL** for Pack 1 Pick 1 (P1P1) discussions on Discord.

Two versions are provided:

* **Simple** — outputs a plain URL
* **Embedded** — outputs a rich embed with an inline pack preview image (recommended)

## ✨ What This Does

Users can run:

```
!p1p1 <CubeID>
```

Example:

```
!p1p1 SGVintageCube
```

Each invocation generates a new CubeCobra sample pack, suitable for immediate P1P1 discussion.

## 🧩 Requirements

* A Discord server with **YAGPDB** installed
* Permission to create **Custom Commands**
* A valid **CubeCobra cube ID**

## ⚙️ Setup Instructions (Common)

1. Open your server’s **YAGPDB control panel**
2. Navigate to **Custom Commands**
3. Create a **new command**
4. Configure:

   * **Trigger type:** `Command`
   * **Trigger:** `p1p1`
   * **Prefix:** your server’s normal prefix (e.g. `!`)
5. Choose **one** of the implementations below

## 📝 Simple Version (Plain URL)

Outputs a single CubeCobra sample pack link.

![Simple command output](p1p1simple.jpg)

### Custom Command

```gotemplate
{{ if lt (len .CmdArgs) 1 }}
Usage: `!p1p1 <CubeCobraCubeID>`
Example: `!p1p1 SGVintageCube`
{{ else }}
{{ $cube := index .CmdArgs 0 }}
{{ $pack := randInt 100000 1000000 }}
https://cubecobra.com/cube/samplepack/{{$cube}}/{{$pack}}
{{ end }}
```

## 🖼️ Embedded Version (With Pack Preview)

Outputs a rich Discord embed including:

* Clickable title
* Cube ID
* Inline preview image of the sample pack
* Attribution footer

**Recommended for active P1P1 discussion.**

![Embedded command output](p1p1embed.jpg)
### Custom Command

```gotemplate
{{ if lt (len .CmdArgs) 1 }}
{{ $e := cembed
  "title" "CubeCobra P1P1"
  "description" "Usage: `!p1p1 <CubeCobraCubeID>`\nExample: `!p1p1 SGVintageCube`"
  "color" 15105570
}}
{{ sendMessage nil $e }}
{{ else }}
{{ $cube := index .CmdArgs 0 }}
{{ $seed := randInt 100000 1000000 }}

{{ $pageURL := printf "https://cubecobra.com/cube/samplepack/%s/%d" $cube $seed }}
{{ $imgURL  := printf "https://cubecobra.com/cube/samplepackimage/%s/%d" $cube $seed }}

{{ $e := cembed
  "title" "Random CubeCobra Sample Pack"
  "url" $pageURL
  "description" (printf "**Cube:** `%s`\nPost your P1P1 in chat." $cube)
  "image" (sdict "url" $imgURL)
  "color" 3066993
  "footer" (sdict "text" (printf "Requested by %s • !p1p1 <CubeID>" .User.Username))
}}
{{ sendMessage nil $e }}
{{ end }}
```

## 🧠 How It Works

* `.CmdArgs` captures the cube ID provided by the user
* `randInt 100000 1000000` generates a random 6-digit seed
  *(upper bound is exclusive in YAGPDB)*
* The seed is used for both:

  * the sample pack page URL
  * the preview image endpoint
* The embedded version explicitly sends the embed using `sendMessage`

## 🔍 Notes

* Only the **first argument** is used
* Cube IDs are **case-sensitive**
* A new pack is generated on every invocation
* The embedded version requires the bot to have **Embed Links** permission

## ☕ Buy Me a Coffee

If you find this useful and want to support my work:

👉 [https://ko-fi.com/bazwtf](https://ko-fi.com/bazwtf)

## 📜 Licence

This project is released into the public domain under **The Unlicense**.

You are free to use, modify, distribute, and do anything you like with it, with or without attribution.

If you want to go even further (very airy layout, narrower sections, or a table-of-contents-friendly version), I can tune the spacing exactly to your taste.
