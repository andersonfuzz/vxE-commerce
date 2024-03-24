# vxE-commerce
-- Criar tabela de Endereços
CREATE TABLE Enderecos (
    address_id INT AUTO_INCREMENT PRIMARY KEY,
    rua VARCHAR(255) NOT NULL,
    cidade VARCHAR(255) NOT NULL,
    estado VARCHAR(100) NOT NULL,
    país VARCHAR(100) NOT NULL,
    cep VARCHAR(20) NOT NULL,
    telefone VARCHAR(20) NOT NULL
);

-- Criar tabela de Usuários
CREATE TABLE Usuarios (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(100) NOT NULL UNIQUE,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    nome VARCHAR(100) NOT NULL,
    sobrenome VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) NOT NULL UNIQUE,  -- Adicionando campo CPF com restrição única
    tipo_usuario ENUM('Moderador', 'ProprietarioLoja', 'Cliente') NOT NULL,
    endereco_id INT NOT NULL,
    data_criacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (endereco_id) REFERENCES Enderecos(address_id)
);

-- Criar tabela de Lojas
CREATE TABLE Lojas (
    store_id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    descricao TEXT NOT NULL,
    endereco_id INT NOT NULL,
    data_criacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (nome),  -- Restrição única para garantir que não haja mais de uma loja com o mesmo nome
    FOREIGN KEY (endereco_id) REFERENCES Enderecos(address_id)
);

-- Criar tabela de Produtos
CREATE TABLE Produtos (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    descricao TEXT,
    preco DECIMAL(10, 2) NOT NULL,
    quantidade_disponivel INT NOT NULL,
    categoria VARCHAR(100) NOT NULL,
    loja_id INT NOT NULL,
    data_criacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (loja_id) REFERENCES Lojas(store_id)
);

-- Criar tabela de Pedidos
CREATE TABLE Pedidos (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    status_pedido ENUM('pendente', 'enviado', 'entregue') NOT NULL,
    data_pedido TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Usuarios(user_id)
);

-- Criar tabela de Itens do Pedido
CREATE TABLE ItensPedido (
    item_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantidade INT NOT NULL,
    preco_unitario DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES Pedidos(order_id),
    FOREIGN KEY (product_id) REFERENCES Produtos(product_id)
);

-- Criar tabela de Carrinho de Compras
CREATE TABLE CarrinhoCompras (
    cart_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    product_id INT NOT NULL,
    quantidade INT NOT NULL,
    data_adicionado TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Usuarios(user_id),
    FOREIGN KEY (product_id) REFERENCES Produtos(product_id)
);

-- Criar tabela de Comentários
CREATE TABLE Comentarios (
    comment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    product_id INT NOT NULL,
    comentario TEXT NOT NULL,
    data_comentario TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Usuarios(user_id),
    FOREIGN KEY (product_id) REFERENCES Produtos(product_id)
);
