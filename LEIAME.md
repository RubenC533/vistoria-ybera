# Visita Técnica Ybera — publicação e envio ao SharePoint

## 1. Fluxo no Power Automate (uma vez só)
1. Criar fluxo **Instant cloud flow** com o gatilho **"Quando uma solicitação HTTP é recebida"**.
   - Esquema do corpo (JSON):
     `{"type":"object","properties":{"fileName":{"type":"string"},"numero":{"type":"string"},"data":{"type":"string"},"local":{"type":"string"},"responsavel":{"type":"string"},"situacao":{"type":"string"},"pdfBase64":{"type":"string"}}}`
   - O app envia `Content-Type: text/plain`; use **Analisar JSON** sobre `json(triggerBody())` com o esquema acima, ou use `json(triggerBody())?['pdfBase64']`.
2. Ação **SharePoint – Criar arquivo**:
   - Endereço do site e pasta: a pasta de destino dos relatórios.
   - Nome do arquivo: `fileName`
   - Conteúdo do arquivo: `base64ToBinary(pdfBase64)`
3. (Opcional) Ação **Resposta** com status 200, para o app confirmar o envio.
4. Salvar e copiar a **URL HTTP POST** gerada.

## 2. Configurar no app
- Tocar em ⚙ e colar a URL, **ou** colar em `ENDPOINT_PADRAO` no `index.html` antes de publicar (assim todos já recebem configurado).
- A URL do fluxo é um segredo: quem a tem consegue gravar na pasta. Publique o app somente em local interno.

## 3. Publicar para tablets e celulares
Câmera e instalação como app exigem **HTTPS**. Opções: SharePoint/Teams não executam HTML,
então use Azure Static Web Apps, um servidor interno com HTTPS ou similar. Publique a pasta inteira
(`index.html`, `manifest.json`, `sw.js`, `icon.svg`, `lib/`).
No tablet: abrir a URL → "Adicionar à tela inicial". Depois da primeira abertura, funciona offline.

Sem internet, o relatório fica salvo no aparelho; o envio é feito depois, ao reabrir a visita.
