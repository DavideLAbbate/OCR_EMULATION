# OCR Text Extractor for Scanned PDFs

Un potente script Python per l'estrazione di testo da PDF scansionati utilizzando OCR (Optical Character Recognition) con funzionalità avanzate di elaborazione delle immagini.

## Caratteristiche Principali

### 🔄 Elaborazione Automatica delle Immagini
- **Rilevamento e correzione dell'orientamento**: Rileva automaticamente se le pagine sono ruotate e le corregge
- **Separazione pagine doppie**: Identifica e separa automaticamente le immagini che contengono due pagine affiancate
- **Rilevamento pagine incomplete**: Identifica e può saltare le pagine tagliate o incomplete

### 🖼️ Miglioramento della Qualità delle Immagini
- **Correzione dell'illuminazione**: Corregge ombreggiature e illuminazione non uniforme
- **Correzione dominanti di colore**: Rimuove colorazioni bluastre o altre dominanti cromatiche
- **Miglioramento qualità**: Ottimizza immagini di bassa qualità (es. foto scattate con telefono)
- **Rimozione rumore**: Elimina piccoli punti neri e artefatti dall'immagine
- **Miglioramento contrasto**: Applica CLAHE per migliorare il contrasto

### 🖐️ Rimozione Elementi Indesiderati
- **Rimozione dita**: Rileva e rimuove dita o altre ostruzioni dalle scansioni
- **Rimozione watermark**: Elimina watermark come "Scanned with CamScanner"
- **Modalità conservativa**: Opzione per rimozione più prudente degli elementi

### 📝 Elaborazione Intelligente del Testo
- **Separazione testo principale e note**: Distingue automaticamente tra testo principale e note a piè di pagina
- **Supporto multilingua**: Include supporto per la lingua italiana
- **Configurazioni OCR multiple**: Prova diverse configurazioni OCR per ottenere i migliori risultati
- **Post-elaborazione**: Pulisce il testo estratto rimuovendo watermark testuali

## Requisiti di Sistema

### Dipendenze Python

#### Installazione Rapida
```bash
pip install -r requirements.txt
```

#### Installazione Manuale (alternativa)
```bash
pip install pytesseract pdf2image opencv-python pillow numpy
```

#### Installazione in Ambiente Virtuale (Raccomandato)
```bash
# Crea un ambiente virtuale
python -m venv ocr_env

# Attiva l'ambiente virtuale
# Su Linux/macOS:
source ocr_env/bin/activate
# Su Windows:
ocr_env\Scripts\activate

# Installa le dipendenze
pip install -r requirements.txt
```

### Dipendenze di Sistema

#### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install tesseract-ocr tesseract-ocr-ita poppler-utils
```

#### macOS
```bash
brew install tesseract tesseract-lang poppler
```

#### Windows
1. Scarica e installa Tesseract da: https://github.com/UB-Mannheim/tesseract/wiki
2. Scarica e installa Poppler da: https://blog.alivate.com.au/poppler-windows/
3. Aggiungi i percorsi alle variabili d'ambiente PATH

## Utilizzo

### Utilizzo Base
```bash
python ocr_to_txt_fixed_content.py documento.pdf
```

Questo comando:
- Elabora `documento.pdf`
- Salva il testo completo in `documento_text.txt`
- Salva il testo principale in `documento_main_text.txt`
- Salva le note in `documento_notes.txt`

### Utilizzo con Opzioni Personalizzate
```bash
python ocr_to_txt_fixed_content.py documento.pdf --output testo_estratto.txt --save-images --image-dir immagini_elaborate
```

### Elaborazione di Più File
```bash
# Elabora tutti i PDF in una cartella
for file in *.pdf; do
    python ocr_to_txt_fixed_content.py "$file"
done
```

```bash
# Script batch per Windows
for %%f in (*.pdf) do python ocr_to_txt_fixed_content.py "%%f"
```

## Parametri della Riga di Comando

### Parametri Obbligatori
- `pdf_path`: Percorso del file PDF da elaborare

### Parametri di Output
- `--output, -o`: Percorso per salvare il testo completo estratto
- `--output-main`: Percorso per salvare solo il testo principale
- `--output-notes`: Percorso per salvare solo le note

### Controllo Elaborazione Immagini
- `--no-auto-orient`: Disabilita il rilevamento automatico dell'orientamento
- `--no-preprocess`: Disabilita la pre-elaborazione delle immagini
- `--no-enhance-contrast`: Disabilita il miglioramento del contrasto
- `--no-quality-enhancement`: Disabilita il miglioramento della qualità

### Correzioni Automatiche
- `--no-lighting-correction`: Disabilita la correzione dell'illuminazione
- `--no-color-correction`: Disabilita la correzione delle dominanti cromatiche
- `--no-noise-removal`: Disabilita la rimozione del rumore

### Rimozione Elementi
- `--no-finger-removal`: Disabilita la rimozione delle dita
- `--aggressive-finger-removal`: Usa rimozione dita più aggressiva
- `--no-watermark-removal`: Disabilita la rimozione dei watermark

### Gestione Pagine
- `--no-skip-incomplete`: Elabora tutte le pagine, anche quelle incomplete
- `--no-split-double`: Non separare le immagini con due pagine
- `--no-separate-notes`: Non separare testo principale e note

### Debug e Sviluppo
- `--save-images`: Salva le immagini elaborate per debug
- `--image-dir`: Cartella per salvare le immagini elaborate

## Esempi di Utilizzo

### Esempio 1: Elaborazione Standard
```bash
python ocr_to_txt_fixed_content.py libro_scansionato.pdf
```
**Output:**
- `libro_scansionato_text.txt` (testo completo)
- `libro_scansionato_main_text.txt` (solo testo principale)
- `libro_scansionato_notes.txt` (solo note)

### Esempio 2: Documento con Problemi di Qualità
```bash
python ocr_to_txt_fixed_content.py foto_documento.pdf --save-images --image-dir debug_images
```
Salva anche tutte le immagini elaborate per verificare i miglioramenti applicati.

### Esempio 3: Documento Semplice (Veloce)
```bash
python ocr_to_txt_fixed_content.py documento_pulito.pdf --no-finger-removal --no-watermark-removal --no-lighting-correction
```
Disabilita le elaborazioni non necessarie per documenti già di buona qualità.

### Esempio 4: Documento con Orientamento Problematico
```bash
python ocr_to_txt_fixed_content.py documento_ruotato.pdf --no-skip-incomplete
```
Elabora tutte le pagine anche se sembrano incomplete.

### Esempio 5: Batch Processing
```bash
# Linux/macOS
find . -name "*.pdf" -exec python ocr_to_txt_fixed_content.py {} \;

# Windows PowerShell
Get-ChildItem -Filter "*.pdf" | ForEach-Object { python ocr_to_txt_fixed_content.py $_.FullName }
```

## Tipi di Input Supportati

### 📄 Documenti Standard
- PDF scansionati da scanner
- PDF creati da foto di documenti
- Documenti con testo stampato
- Libri e riviste scansionati

### 📱 Foto di Documenti
- Foto scattate con smartphone
- Immagini con illuminazione non uniforme
- Documenti fotografati con angolazioni diverse
- Immagini con dita o ostruzioni parziali

### 📚 Documenti Accademici
- Articoli con note a piè di pagina
- Libri con annotazioni
- Documenti con layout complessi
- Pagine doppie scansionate insieme

### 🏢 Documenti Aziendali
- Contratti scansionati
- Fatture e documenti amministrativi
- Presentazioni convertite in PDF
- Documenti con watermark

## Struttura dei File di Output

### File Testo Completo (`*_text.txt`)
```
--- PAGE 1 ---
[Testo della pagina 1]

--- NOTES ---
[Note della pagina 1]

--- PAGE 2 ---
[Testo della pagina 2]
...
```

### File Testo Principale (`*_main_text.txt`)
```
--- PAGE 1 ---
[Solo il testo principale della pagina 1]

--- PAGE 2 ---
[Solo il testo principale della pagina 2]
...
```

### File Note (`*_notes.txt`)
```
--- PAGE 1 NOTES ---
[Solo le note della pagina 1]

--- PAGE 2 NOTES ---
[Solo le note della pagina 2]
...
```

## Risoluzione Problemi

### Errore: "TesseractNotFoundError"
**Soluzione:** Installa Tesseract e assicurati che sia nel PATH di sistema.

### Errore: "pdf2image conversion failed"
**Soluzione:** Installa Poppler e assicurati che sia nel PATH di sistema.

### Testo Estratto di Bassa Qualità
**Soluzioni:**
- Usa `--save-images` per verificare le elaborazioni
- Prova senza `--no-preprocess`
- Abilita tutte le correzioni automatiche

### Pagine Ruotate Incorrettamente
**Soluzioni:**
- Il sistema prova solo rotazioni di 0° e 180° per evitare errori
- Verifica manualmente l'orientamento delle immagini salvate

### Rimozione Eccessiva di Contenuto
**Soluzioni:**
- Usa `--aggressive-finger-removal` solo se necessario
- Disabilita `--no-finger-removal` per documenti puliti

## Prestazioni e Ottimizzazione

### Tempi di Elaborazione Tipici
- **Documento semplice (10 pagine):** 2-5 minuti
- **Documento complesso (50 pagine):** 10-25 minuti
- **Foto di bassa qualità:** +50% tempo aggiuntivo

### Suggerimenti per Migliorare le Prestazioni
1. Disabilita le funzionalità non necessarie per documenti puliti
2. Usa `--no-save-images` per elaborazioni di produzione
3. Elabora file più piccoli quando possibile

## Limitazioni

- Funziona meglio con testo stampato (non manoscritto)
- Richiede immagini con risoluzione sufficiente (>150 DPI)
- Le lingue supportate dipendono dai pacchetti Tesseract installati
- La separazione automatica delle note potrebbe non essere perfetta per tutti i layout

## Contributi e Sviluppo

Per miglioramenti o segnalazioni di bug, considera:
- Test con diversi tipi di documenti
- Ottimizzazione dei parametri OCR
- Supporto per lingue aggiuntive
- Miglioramento degli algoritmi di rilevamento

## Licenza

Questo script utilizza librerie open source. Verifica le licenze delle dipendenze per uso commerciale.
