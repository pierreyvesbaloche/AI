# Procédure d'installation de Stable diffusion en local

## Installation

### Stable Diffusion (SD) / Automatic 1111

- Accéder à https://github.com/AUTOMATIC1111/stable-diffusion-webui
- Consulter le fichier d'installation en fonction de la plateforme (NVIDIA ou autre) : https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/README.md
- Dans un cas NVIDIA, Télécharger l'archive tout en un : https://github.com/AUTOMATIC1111/stable-diffusion-webui/releases/tag/v1.0.0-pre
- Extraire le contenu de l'archive
- Effectuer la mise à jour en lançant le fichier `update.bat`

### Modèles à télécharger

Télécharger les modèles suivants et les placer dans le sous-répertoire de l'arborescence (`webui\models\Stable-diffusion`)

- Rev animated : https://civitai.com/models/7371?modelVersionId=46846
	- Fichier : revAnimated_v122EOL.safetensors (5.3 Go)
- Real Dream : https://civitai.com/models/153568?modelVersionId=712448
	- Fichier : realDream_15SD15.safetensors (2 Go)
- RealCartoon-Realistic	: https://civitai.com/models/97744?modelVersionId=671503
	- Fichier : realcartoonRealistic_v17.safetensors (6 Go)

### Styles supplémentaires

- Télécharger le fichier suivant : https://drive.usercontent.google.com/download?id=1pJIcFqLLR8CyhR7QYCMP93UbFe_pu2Zc&export=download&authuser=0
- Le placer dans `\webui`

## Paramétrage de Stable Diffusion

- Editer le fichier d'exécution `webui\webui-user.bat`
- Configurer les paramètres suivants : `set COMMANDLINE_ARGS= --autolaunch --theme dark --xformers --api`
- Dans la fenêtre de l'éditeur, dans l'onglet `txt2img`, toujours avoir : 
  - Sampling method : `DPM++ 2M`
  - Schedule Type : `Karras`
  - Sampling Steps : `30`
  - Activer Refiner et choisir CFG Scale : `7`
	
## Extensions à télécharger 

### Reactor - Remplacement de visage
- Page principale : https://github.com/Gourieff/sd-webui-reactor

- Procédure d'installation :
  1. Lancer SD avec `run.bat`.
  1. Sélectionner l'onglet `Extensions`.
  1. Choisir `Install from URL`.
  1. Copier l'URL suivante : `https://github.com/Gourieff/sd-webui-reactor`.
  1. Cliquer sur le bouton `Install`.
  1. Sur l'onglet `Installed`, une nouvelle ligne avec `sd-webui-reactor` doit être présente.
  1. Redémarrer SD.
  
- Paramétrage Global par Génération
  1. Sélectionner l'onglet `txt2img`.
  1. Cocher la case `ReActor`.
  1. Déplier le toggle de fin de ligne.
  1. Sélectionner une image de visage.

## Gestion des API 
- Wiki de référence : https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API
- Endpoint Local : http://127.0.0.1:7860/docs#/default/text2imgapi_sdapi_v1_txt2img_post

### Exemple avec Reactor 
- Une fois une image uploadée via l'IHM de SD pour le visage à utiliser, récupérer via les Google Dev Tools le contenu en base64
- Compléter l'exemple ci-dessous (via POST du Swagger) avec le contenu de l'image :
```json
{
  "prompt": "MY-GREAT-PROMPT-PLUS-THE-FOLLOWING photograph (full body:0.6) (portrait:0.7) no crop, cinematic lighting, golden ratio, perfect composition, sharp focus on closeup, masterpiece, cinematic, 4k, high quality, (photorealistic:1.1)",
  "negative_prompt": "ugly, poorly drawn face, extra limbs, deformed, blurry, nude",
  "seed": -1,
  "subseed": -1,
  "subseed_strength": 0,
  "width": 512, 
  "height": 512,
  "sampler_name": "DPM++ 2M",
  "scheduler": "Karras",
  "cfg_scale": 7,
  "steps": 30,
  "batch_size": 1,
  "restore_faces": false,
  "denoising_strength": 0.7,
  "send_images": false,
  "save_images": true,
  "alwayson_scripts" : {
	"reactor" : {"args" : [
		"data:image/jpeg;base64,PASTE-HERE-BASE64-CONTENT", 
		true, 
		"0", 
		"0",
		"inswapper_128.onnx",
		"CodeFormer",
		1,
		true,
		"None",
		2,
		1,
		false,
		true,
		1,
		0,
		0,
		false,
		0.5,
		true,
		false,
		"CUDA",
		false,
		0,
		"",
		"",
		null,
		true,
		true,
		0.6,
		1
	]
	}
  }
}
```

