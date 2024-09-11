from twilio.rest import Client  # Biblioteca Twilio para WhatsApp
import logging
import mercadopago  # Biblioteca Mercado Pago

# Configurações do Twilio e Mercado Pago
TWILIO_ACCOUNT_SID = "AC71b1f4f930608343f2205b338c3a61fb"
TWILIO_AUTH_TOKEN = "6ea23469ac026321bf4f243c598dbea6"
TWILIO_WHATSAPP_NUMBER = "whatsapp:+5548988224533"  # Número do WhatsApp Sandbox Twilio
ADMIN_WHATSAPP_NUMBER = "whatsapp:+5548988224533"  # Número verificado do admin

CHAVE_PIX_FIXA = "sua_chave_pix_aqui"
MERCADO_PAGO_EMAIL = "seu_email_mercadopago@gmail.com"
ACCESS_TOKEN_MP = "SEU_ACCESS_TOKEN_DO_MERCADO_PAGO"
LINK_CATALOGO = "https://wa.me/c/554896605599"  # Link do catálogo do WhatsApp Business

# Inicializa o Mercado Pago e Twilio
mp = mercadopago.SDK(ACCESS_TOKEN_MP)
client = Client(TWILIO_ACCOUNT_SID, TWILIO_AUTH_TOKEN)

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# Aqui você define seus produtos e categorias manualmente
catalogo = {
    "eletronicos": [
        {"nome": "Smartphone XYZ", "descricao": "Smartphone com 128GB de armazenamento.", "preco": 1999.90, "link": "https://wa.me/p/1234567890"},
        {"nome": "Notebook ABC", "descricao": "Notebook com processador i7.", "preco": 3599.00, "link": "https://wa.me/p/0987654321"}
    ],
    "moda": [
        {"nome": "Tênis Running", "descricao": "Tênis de corrida confortável.", "preco": 299.99, "link": "https://wa.me/p/1122334455"},
        {"nome": "Relógio Digital", "descricao": "Relógio com display LED.", "preco": 199.90, "link": "https://wa.me/p/6677889900"}
    ]
}

# Função para enviar mensagens pelo WhatsApp
def enviar_mensagem(numero, mensagem):
    client.messages.create(
        body=mensagem,
        from_=TWILIO_WHATSAPP_NUMBER,
        to=numero
    )

# Função de boas-vindas
def start(numero):
    mensagem = "Bem-vindo à Innova! Use 'listar' para ver os produtos disponíveis ou 'catalogo' para ver nosso catálogo completo."
    enviar_mensagem(numero, mensagem)

# Função para listar produtos de uma categoria
def listar_produtos(numero, categoria):
    if categoria not in catalogo:
        enviar_mensagem(numero, "Categoria não encontrada.")
        return
    
    mensagem = f"Produtos da categoria {categoria.capitalize()}:\n\n"
    for idx, produto in enumerate(catalogo[categoria], start=1):
        mensagem += f"{idx}. {produto['nome']}\n{produto['descricao']}\nPreço: R$ {produto['preco']:.2f}\n[Ver mais]({produto['link']})\n\n"

    enviar_mensagem(numero, mensagem)

# Função para exibir o link do catálogo completo
def exibir_catalogo(numero):
    mensagem = f"Veja nosso catálogo completo aqui: {LINK_CATALOGO}"
    enviar_mensagem(numero, mensagem)

# Função principal para processar as mensagens recebidas
def receber_mensagem(whatsapp_numero, mensagem):
    if mensagem.lower() == 'start':
        start(whatsapp_numero)
    elif mensagem.lower() == 'listar eletronicos':
        listar_produtos(whatsapp_numero, 'eletronicos')
    elif mensagem.lower() == 'listar moda':
        listar_produtos(whatsapp_numero, 'moda')
    elif mensagem.lower() == 'catalogo':
        exibir_catalogo(whatsapp_numero)
    else:
        enviar_mensagem(whatsapp_numero, "Comando não reconhecido. Use 'listar' seguido da categoria ou 'catalogo' para ver o catálogo completo.")

# Simulação de recebimento de mensagens (essa função normalmente será chamada por um webhook que você configurar no Twilio)
def main():
    numero_teste = ADMIN_WHATSAPP_NUMBER  # Número de teste
    receber_mensagem(numero_teste, "start")  # Exemplo de teste com o comando 'start'
    receber_mensagem(numero_teste, "listar eletronicos")  # Exemplo de listagem de produtos
    receber_mensagem(numero_teste, "catalogo")  # Exemplo de exibição do catálogo

if __name__ == "__main__":
    main()

