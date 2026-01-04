# Legfontosabb AI alkalmazások
[IA összefoglaló](https://gemini.google.com/share/0a36b2a649ec)
[aistudio](https://aistudio.google.com/)


# Gemini API
Az AI Studio egyik fő célja, hogy ne csak beszélgess az AI-val, hanem építsd is be a saját weboldaladba vagy szoftveredbe: Vedd a Gemini modellt, küldd el neki ezt a kérdést, és írd ki a választ a képernyőre.

- npm modul:
```console
npm install @google/generative-ai
```

## Egyszerű kérés-válasz
```js
import { GoogleGenAI } from "@google/genai";

const ai = new GoogleGenAI({});

async function main() {
  const response = await ai.models.generateContent({
    model: "gemini-3-pro-preview",
    contents: "Explain how AI works in a few words",
  });
  console.log(response.text);
}

await main();
```


## kérés-követett válasz


```js
import { GoogleGenAI } from "@google/genai";

const ai = new GoogleGenAI({
  apiKey: "IDE_MÁSOLD_AZ_API_KULCSODAT"
});

async function main() {
  // A generateContentStream függvényt használjuk a sima helyett
  const response = await ai.models.generateContentStream({
    model: "gemini-2.0-flash", // Használhatod a legújabb modellt
    contents: "Írj egy 200 szavas mesét egy robotról, aki sütit akar sütni.",
  });

  process.stdout.write("AI válasza: ");

  // Végigmegyünk a beérkező darabkákon (chunk-okon)
  for await (const chunk of response) {
    // Kiírjuk a darabkát a terminálba anélkül, hogy új sort kezdene
    process.stdout.write(chunk.text);
  }
  
  console.log("\n\n--- Generálás befejezve ---");
}

main();
}

run();
```