from IPython.utils.text import strip_ansi
from IPython.core import splitinput
from bs4.element import Script
import requests
from bs4 import BeautifulSoup

def extract_text_from_url(url):
    response = requests.get(url)

    if response.status_code == 200:
        soup = BeautifulSoup(response.text, 'html.parser')
        for script_or_style in soup(['script', 'style']):
            script_or_style.decompose()

        # Get the text after removing all script and style tags
        texto = soup.get_text(separator=' ')

        # Limpar texto
        linhas = (line.strip() for line in texto.splitlines())
        parts = (phrase.strip() for line in linhas for phrase in line.split(" "))
        # Corrected 'if parts' to 'if part' to filter out empty strings
        texto_limpo = '\n'.join(part for part in parts if part)

        return texto_limpo
    else:
        print(f"Failed to fetch the URL. Status code: {response.status_code}")
        return None

    # These lines were unreachable and redundant, so they are removed.
    # text = soup.get_text()
    # return text

extract_text_from_url('https://dev.to/devteam/github-copilot-cli-challenge-badges-are-here-28-chances-to-win-a-prize-3c42')
*****______________________________________________________________
from langchain_openai.chat_models.azure import AzureChatOpenAI

client = AzureChatOpenAI(
    azure_endpoint= "https://helle-ml1lekc7-swedencentral.cognitiveservices.azure.com/",
    api_key= "xxxxxxxxxxx",
    api_version= "2024-12-01-preview",
    deployment_name= "gpt-5.2-chat",
    max_retries=0
)

def translate_article(text, lang):
  messages = [
      ("system", "Você atua como tradutor de textos"),
      ("user", f"Traduza o {text} para o idioma {lang} e responda em markdown")
  ]

  response = client.invoke(messages)
  # Removed print(response.content) from here to separate concerns
  return response.content

# Explicitly print the result of the function call
print(translate_article("GitHub Copilot CLI Challenge Badges Are Here + 28 Chances to Win a Prize!","portugues"))

*****___________________________________________________________________________

url = 'https://dev.to/devteam/github-copilot-cli-challenge-badges-are-here-28-chances-to-win-a-prize-3c42'
text = extract_text_from_url(url)
article = translate_article(text, "bt-br")
print(article)
