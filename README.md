# Projeto-para-redu-o-de-dimensionalidade-em-imagens
Conversão de Imagem Colorida para Tons de Cinza e Binarização em Python

Objetivo
O objetivo deste projeto é converter uma imagem colorida (qualquer formato comum como .jpg, .png) em: uma imagem em tons de cinza (valores de 0 a 255); e uma imagem binarizada (preto e branco) com valores 0 e 255.

Todo o processamento é feito sem o uso de bibliotecas de visão computacional (como OpenCV ou NumPy).
Apenas o Pillow (PIL) é utilizado para abrir, criar e salvar imagens, o que é permitido, já que o foco do exercício está no processamento manual dos pixels.

Ferramentas utilizadas
Google Colab — ambiente de execução em nuvem, ideal para testes em Python;
Python 3 — linguagem de programação utilizada;
Pillow (PIL) — biblioteca usada apenas para leitura e escrita de imagens.

Estrutura geral do processo
Etapa	Descrição	Resultado
1️⃣	Upload da imagem	Carregamento do arquivo de imagem do computador
2️⃣	Leitura da imagem	Conversão em lista de pixels RGB
3️⃣	Conversão para cinza	Cálculo manual dos níveis de cinza (0–255)
4️⃣	Conversão para binário	Aplicação de limiar (threshold) → 0 ou 255
5️⃣	Criação de novas imagens	Geração de arquivos .png com os novos valores
6️⃣	Exibição	Visualização das três versões no Colab
7️⃣	(Opcional) Download	Download dos arquivos processados
🧱 Passo a Passo no Google Colab

1. Fazer upload da imagem
from google.colab import files

# Faz upload da imagem (JPG, PNG, etc.)
uploaded = files.upload()

# Guarda o nome do arquivo
nome_arquivo = list(uploaded.keys())[0]
print("Imagem carregada:", nome_arquivo)

Após executar essa célula, clique em “Escolher arquivo” e selecione a imagem do seu computador.

2. Ler a imagem e obter os pixels RGB
from PIL import Image

# Abre a imagem e converte para RGB
img = Image.open(nome_arquivo).convert('RGB')
largura, altura = img.size
pixels = list(img.getdata())

print(f"📸 Imagem carregada: {largura}x{altura} pixels")


Aqui a imagem é aberta e transformada em uma lista de tuplas do tipo (R, G, B).

Exemplo:

[(255, 0, 0), (120, 200, 50), (0, 0, 0), ...]

3. Converter para tons de cinza (0–255)
def converter_para_cinza(pixels):
    """
    Converte RGB → tons de cinza (0 a 255)
    Fórmula perceptiva: Y = 0.299R + 0.587G + 0.114B
    """
    tons_cinza = []
    for r, g, b in pixels:
        y = int(0.299*r + 0.587*g + 0.114*b)
        tons_cinza.append((y, y, y))
    return tons_cinza

cinza = converter_para_cinza(pixels)

Explicação da fórmula:

O olho humano é mais sensível à luz verde → por isso o peso de 0.587;

O vermelho e azul têm pesos menores (0.299 e 0.114).
Essa fórmula é a padrão da TV e da fotografia digital (luminância Y).

 4. Converter para imagem binária (preto e branco)
def converter_para_binario(pixels, limiar=128):
    """
    Converte tons de cinza → preto e branco (0 ou 255)
    """
    binario = []
    for r, g, b in pixels:
        valor = 255 if r >= limiar else 0
        binario.append((valor, valor, valor))
    return binario

binaria = converter_para_binario(cinza)


Lógica do limiar (threshold):

Se o valor do pixel ≥ 128 → pixel vira branco (255)

Se o valor do pixel < 128 → pixel vira preto (0)

O valor 128 é o meio de 0–255, mas pode ser alterado (por exemplo, 100 ou 150) para obter resultados diferentes.

5. Criar novas imagens e salvar
img_cinza = Image.new('RGB', (largura, altura))
img_cinza.putdata(cinza)
img_cinza.save("imagem_cinza.png")

img_binaria = Image.new('RGB', (largura, altura))
img_binaria.putdata(binaria)
img_binaria.save("imagem_binaria.png")

print("Imagens geradas com sucesso!")

6. Exibir as imagens no Colab
from IPython.display import Image as ShowImage, display

print("🎨 Imagem original:")
display(ShowImage(nome_arquivo))

print("🌫️ Tons de cinza:")
display(ShowImage("imagem_cinza.png"))

print("⬛⬜ Imagem binarizada:")
display(ShowImage("imagem_binaria.png"))


Aqui serão exibidas:

A imagem original colorida;

A versão em tons de cinza;

A versão binarizada (preto e branco).

7. (Opcional) Baixar os resultados
from google.colab import files

files.download("imagem_cinza.png")
files.download("imagem_binaria.png")


Assim você pode salvar os resultados processados no seu computador.

Resultado esperado
Tipo de Imagem	Descrição	Escala de valores
Original	Colorida	RGB (0–255, 0–255, 0–255)
Tons de cinza	Intensidade luminosa	0–255
Binarizada	Preto e branco	0 ou 255

Conclusão
Este projeto demonstrou como realizar processamento de imagem em baixo nível, entendendo como cada pixel é representado e manipulado numericamente.
Foram abordados conceitos fundamentais de Visão Computacional:

Representação RGB;

Luminância e percepção visual;

Limiarização (thresholding);

Conversão manual de pixels e manipulação de listas.

Mesmo sem o uso de bibliotecas complexas, é possível reproduzir etapas essenciais de pré-processamento de imagem, aplicáveis em sistemas de classificação, reconhecimento ou segmentação.

Autor: Jhon Rodrigues
Novembro 2025
