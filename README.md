# SLQ_banco_de_dados_pizzaria
uma banco que desenvolvi só com base em um outro que desenvolvi em grupo na faculdade em grupo
CREATE DATABASE PIZZARIA_2;
USE PIZZARIA_2;

CREATE TABLE IF NOT EXISTS `cliente` (
	`id_cliente` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`nome_completo` VARCHAR(150) NOT NULL,
	`observacao` TEXT,
	`cpf_cnpj` VARCHAR(18) UNIQUE,
	`email` VARCHAR(150),
	`data_cadastro` DATETIME NOT NULL,
	`ativo` BOOLEAN NOT NULL DEFAULT 1,
	PRIMARY KEY(`id_cliente`)
);


CREATE TABLE IF NOT EXISTS `cliente_telefone` (
	`id_cliente_telefone` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_cliente` INT NOT NULL,
	`numero` VARCHAR(20) NOT NULL,
	`principal` BOOLEAN NOT NULL DEFAULT 0,
	PRIMARY KEY(`id_cliente_telefone`)
);


CREATE TABLE IF NOT EXISTS `endereco` (
	`id_endereco` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_cliente` INT NOT NULL,
	`id_bairro` INT NOT NULL,
	`rua` VARCHAR(150) NOT NULL,
	`numero` VARCHAR(10) NOT NULL,
	`complemento` VARCHAR(100),
	`cep` VARCHAR(9) NOT NULL,
	`ponto_referencia` VARCHAR(255),
	`principal` BOOLEAN NOT NULL DEFAULT 0,
	PRIMARY KEY(`id_endereco`)
);


CREATE TABLE IF NOT EXISTS `bairro` (
	`id_bairro` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_zona` INT NOT NULL,
	`nome` VARCHAR(100) NOT NULL,
	`taxa_entrega` DECIMAL(10,2) NOT NULL DEFAULT '0.00',
	PRIMARY KEY(`id_bairro`)
);


CREATE TABLE IF NOT EXISTS `zona_entrega` (
	`id_zona_entrega` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(100) NOT NULL,
	PRIMARY KEY(`id_zona_entrega`)
);


CREATE TABLE IF NOT EXISTS `categoria_produto` (
	`id_categoria_produto` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(100) NOT NULL,
	PRIMARY KEY(`id_categoria_produto`)
);


CREATE TABLE IF NOT EXISTS `produto` (
	`id_produto` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_categoria` INT NOT NULL,
	`nome` VARCHAR(150) NOT NULL,
	`preco_base` DECIMAL(10,2) NOT NULL DEFAULT '0.00',
	`disponivel` BOOLEAN NOT NULL DEFAULT 1,
	`descricao` TEXT,
	`tempo_preparo` INT COMMENT 'minutos',
	`criado_em` DATETIME NOT NULL,
	`atualizado_em` DATETIME,
	PRIMARY KEY(`id_produto`)
);


CREATE TABLE IF NOT EXISTS `pizza` (
	`id_pizza` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`nome` VARCHAR(150) NOT NULL,
	`disponivel` BOOLEAN NOT NULL DEFAULT 1,
	PRIMARY KEY(`id_pizza`)
);


CREATE TABLE IF NOT EXISTS `tamanho_pizza` (
	`id_tamanho_pizza` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(50) NOT NULL UNIQUE,
	PRIMARY KEY(`id_tamanho_pizza`)
);


CREATE TABLE IF NOT EXISTS `pizza_tamanho` (
	`id_pizza_tamanho` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_pizza` INT NOT NULL,
	`id_tamanho` INT NOT NULL,
	`preco` DECIMAL(10,2) NOT NULL DEFAULT '0.00',
	PRIMARY KEY(`id_pizza_tamanho`)
);


CREATE TABLE IF NOT EXISTS `sabor_pizza` (
	`id_sabor_pizza` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`nome` VARCHAR(100) NOT NULL,
	`preco_base` DECIMAL(10,2) NOT NULL DEFAULT '0.00',
	PRIMARY KEY(`id_sabor_pizza`)
);


CREATE TABLE IF NOT EXISTS `pizza_sabor` (
	`id_pizza_sabor` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_sabor` INT NOT NULL,
	`id_pizza` INT NOT NULL,
	PRIMARY KEY(`id_pizza_sabor`)
);


CREATE TABLE IF NOT EXISTS `borda` (
	`id_borda` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(100) NOT NULL,
	`valor_adicional` DECIMAL(10,2) NOT NULL DEFAULT '0.00',
	PRIMARY KEY(`id_borda`)
);


CREATE TABLE IF NOT EXISTS `adicional` (
	`id_adicional` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(100) NOT NULL,
	`valor_adicional` DECIMAL(10,2) NOT NULL DEFAULT '0.00',
	PRIMARY KEY(`id_adicional`)
);


CREATE TABLE IF NOT EXISTS `promocao` (
	`id_promocao` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(150) NOT NULL,
	`valor_desconto` DECIMAL(10,2) NOT NULL,
	`data_inicio` DATETIME NOT NULL,
	`data_fim` DATETIME,
	`tipo_desconto` VARCHAR(20) NOT NULL DEFAULT '''valor''' COMMENT 'valor | percentual',
	`ativo` BOOLEAN NOT NULL DEFAULT 1,
	PRIMARY KEY(`id_promocao`)
);


CREATE TABLE IF NOT EXISTS `promocao_produto` (
	`id_promocao_produto` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_produto` INT NOT NULL,
	`id_promocao` INT NOT NULL,
	PRIMARY KEY(`id_promocao_produto`)
);


CREATE TABLE IF NOT EXISTS `ingrediente` (
	`id_ingrediente` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`nome` VARCHAR(100) NOT NULL,
	`disponivel` BOOLEAN NOT NULL DEFAULT 1,
	PRIMARY KEY(`id_ingrediente`)
);


CREATE TABLE IF NOT EXISTS `produto_ingrediente` (
	`id_produto_ingrediente` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_ingrediente` INT NOT NULL,
	`quantidade` DECIMAL(10,3) NOT NULL,
	`id_produto` INT NOT NULL,
	PRIMARY KEY(`id_produto_ingrediente`)
);


CREATE TABLE IF NOT EXISTS `estoque` (
	`id_estoque` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_ingrediente` INT NOT NULL UNIQUE,
	`quantidade_atual` DECIMAL(10,3) NOT NULL DEFAULT '0.000',
	`estoque_minimo` DECIMAL(10,3) NOT NULL DEFAULT '0' COMMENT 'alerta de reposição',
	PRIMARY KEY(`id_estoque`)
);


CREATE TABLE IF NOT EXISTS `movimentacao_estoque` (
	`id_movimentacao_estoque` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_estoque` INT NOT NULL,
	`tipo` VARCHAR(20) NOT NULL,
	`quantidade` DECIMAL(10,3) NOT NULL,
	`data_hora` DATETIME NOT NULL,
	`id_compra` INT,
	PRIMARY KEY(`id_movimentacao_estoque`)
);


CREATE TABLE IF NOT EXISTS `fornecedor` (
	`id_fornecedor` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`nome` VARCHAR(150) NOT NULL,
	`telefone` VARCHAR(20),
	PRIMARY KEY(`id_fornecedor`)
);


CREATE TABLE IF NOT EXISTS `compra` (
	`id_compra` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_fornecedor` INT NOT NULL,
	`data_compra` DATETIME NOT NULL,
	PRIMARY KEY(`id_compra`)
);


CREATE TABLE IF NOT EXISTS `item_compra` (
	`id_item_compra` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_compra` INT NOT NULL,
	`id_ingrediente` INT NOT NULL,
	`quantidade` DECIMAL(10,3) NOT NULL,
	`valor_unitario` DECIMAL(10,2) NOT NULL,
	PRIMARY KEY(`id_item_compra`)
);


CREATE TABLE IF NOT EXISTS `pedido` (
	`id_pedido` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_cliente` INT NOT NULL,
	`id_mesa` INT COMMENT 'nulo para pedidos de delivery',
	`tipo_pedido` VARCHAR(20) NOT NULL,
	`data_hora` DATETIME NOT NULL,
	`valor_total` DECIMAL(10,2) NOT NULL,
	`id_promocao` INT,
	`numero_pedido` VARCHAR(20) NOT NULL UNIQUE COMMENT 'código visível ao cliente',
	`id_endereco` INT COMMENT 'destino do delivery; nulo em consumo no local',
	`id_funcionario` INT COMMENT 'quem registrou o pedido',
	`subtotal` DECIMAL(10,2) NOT NULL DEFAULT '0',
	`valor_desconto` DECIMAL(10,2) NOT NULL DEFAULT '0',
	`taxa_entrega` DECIMAL(10,2) NOT NULL DEFAULT '0',
	`criado_em` DATETIME NOT NULL,
	`atualizado_em` DATETIME,
	PRIMARY KEY(`id_pedido`)
);


CREATE TABLE IF NOT EXISTS `status_pedido` (
	`id_status` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(100) NOT NULL,
	PRIMARY KEY(`id_status`)
);


CREATE TABLE IF NOT EXISTS `historico_status` (
	`id_historico` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_pedido` INT NOT NULL,
	`id_status` INT NOT NULL,
	`data_hora` DATETIME NOT NULL,
	PRIMARY KEY(`id_historico`)
);


CREATE TABLE IF NOT EXISTS `item_pedido` (
	`id_item_pedido` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_pedido` INT NOT NULL,
	`id_produto` INT COMMENT 'nulo quando o item é uma pizza',
	`quantidade` INT NOT NULL,
	`valor_unitario` DECIMAL(10,2) NOT NULL,
	`id_pizza` INT,
	PRIMARY KEY(`id_item_pedido`)
);


CREATE TABLE IF NOT EXISTS `pizza_pedido` (
	`id_pizza_pedido` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_item_pedido` INT NOT NULL,
	`id_tamanho` INT NOT NULL,
	`id_borda` INT,
	`observacao_preparo` VARCHAR(255),
	PRIMARY KEY(`id_pizza_pedido`)
);


CREATE TABLE IF NOT EXISTS `pizza_pedido_sabor` (
	`id_pizza_pedido_sabor` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_sabor` INT NOT NULL,
	`id_pizza_pedido` INT NOT NULL,
	PRIMARY KEY(`id_pizza_pedido_sabor`)
);


CREATE TABLE IF NOT EXISTS `item_pedido_adicional` (
	`id_item_adicional` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_item_pedido` INT NOT NULL,
	`id_adicional` INT NOT NULL,
	`quantidade` INT NOT NULL DEFAULT '1',
	PRIMARY KEY(`id_item_adicional`)
);


CREATE TABLE IF NOT EXISTS `mesa` (
	`id_mesa` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`numero_mesa` VARCHAR(10) NOT NULL UNIQUE,
	`status` VARCHAR(20) NOT NULL DEFAULT '''livre''' COMMENT 'livre | ocupada | reservada | inativa',
	PRIMARY KEY(`id_mesa`)
);


CREATE TABLE IF NOT EXISTS `observacao_pedido` (
	`id_observacao` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_pedido` INT NOT NULL,
	`descricao` TEXT NOT NULL,
	PRIMARY KEY(`id_observacao`)
);


CREATE TABLE IF NOT EXISTS `motoboy` (
	`id_motoboy` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`nome` VARCHAR(150) NOT NULL,
	`telefone` VARCHAR(20),
	`tipo_vinculo` VARCHAR(30) NOT NULL,
	`veiculo` VARCHAR(50),
	`placa` VARCHAR(10),
	`ativo` BOOLEAN NOT NULL DEFAULT 1,
	PRIMARY KEY(`id_motoboy`)
);


CREATE TABLE IF NOT EXISTS `entrega` (
	`id_entrega` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_pedido` INT NOT NULL,
	`id_motoboy` INT,
	`horario_saida` DATETIME,
	`horario_retorno` DATETIME,
	`estimativa_tempo` INT COMMENT 'minutos',
	`id_endereco` INT NOT NULL COMMENT 'destino real da entrega',
	`taxa_entrega` DECIMAL(10,2) NOT NULL DEFAULT '0',
	`status` VARCHAR(20) NOT NULL DEFAULT '''pendente''',
	`criado_em` DATETIME NOT NULL,
	`observacao` VARCHAR(255),
	PRIMARY KEY(`id_entrega`)
);


CREATE TABLE IF NOT EXISTS `escala_motoboy` (
	`id_escala` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_motoboy` INT NOT NULL,
	`data_escala` DATE NOT NULL,
	PRIMARY KEY(`id_escala`)
);


CREATE TABLE IF NOT EXISTS `rota_entrega` (
	`id_rota` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_entrega` INT NOT NULL,
	`observacao` VARCHAR(255),
	PRIMARY KEY(`id_rota`)
);


CREATE TABLE IF NOT EXISTS `comissao_motoboy` (
	`id_comissao` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_motoboy` INT NOT NULL,
	`valor` DECIMAL(10,2) NOT NULL,
	`data_referencia` DATE NOT NULL,
	PRIMARY KEY(`id_comissao`)
);


CREATE TABLE IF NOT EXISTS `forma_pagamento` (
	`id_forma_pagamento` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(50) NOT NULL,
	PRIMARY KEY(`id_forma_pagamento`)
);


CREATE TABLE IF NOT EXISTS `pagamento` (
	`id_pagamento` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_pedido` INT NOT NULL,
	`id_forma_pagamento` INT NOT NULL,
	`valor` DECIMAL(10,2) NOT NULL,
	`data_hora` DATETIME NOT NULL,
	`status` VARCHAR(20) NOT NULL DEFAULT '''pendente''',
	`id_caixa` INT COMMENT 'caixa/turno onde o recebimento foi lançado',
	PRIMARY KEY(`id_pagamento`)
);


CREATE TABLE IF NOT EXISTS `bandeira` (
	`id bandeira` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`nome` VARCHAR(50) NOT NULL,
	PRIMARY KEY(`id bandeira`)
);


CREATE TABLE IF NOT EXISTS `pagamento_cartao` (
	`id_pagamento` INT NOT NULL,
	`id_bandeira` INT NOT NULL,
	`tipo_cartao` VARCHAR(20) NOT NULL,
	PRIMARY KEY(`id_pagamento`)
);


CREATE TABLE IF NOT EXISTS `pagamento_pix` (
	`id_pagamento` INT NOT NULL,
	`codigo_transacao` VARCHAR(100) NOT NULL,
	PRIMARY KEY(`id_pagamento`)
);


CREATE TABLE IF NOT EXISTS `troco` (
	`id_troco` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_pagamento` INT NOT NULL,
	`troco_para` DECIMAL(10,2) NOT NULL,
	PRIMARY KEY(`id_troco`)
);


CREATE TABLE IF NOT EXISTS `funcionario` (
	`id_funcionario` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`nome` VARCHAR(150) NOT NULL,
	`id_cargo` INT NOT NULL,
	`cpf` VARCHAR(14) UNIQUE,
	`telefone` VARCHAR(20),
	`data_admissao` DATE,
	`ativo` BOOLEAN NOT NULL DEFAULT 1,
	PRIMARY KEY(`id_funcionario`)
);


CREATE TABLE IF NOT EXISTS `cargo` (
	`id_cargo` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(100) NOT NULL,
	PRIMARY KEY(`id_cargo`)
);


CREATE TABLE IF NOT EXISTS `usuario_sistema` (
	`id_usuario` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_funcionario` INT NOT NULL,
	`login` VARCHAR(50) NOT NULL UNIQUE,
	`senha` VARCHAR(255) NOT NULL,
	`id_perfil` INT NOT NULL,
	`ativo` BOOLEAN NOT NULL DEFAULT 1,
	`ultimo_acesso` DATETIME,
	PRIMARY KEY(`id_usuario`)
);


CREATE TABLE IF NOT EXISTS `perfil_acesso` (
	`id_perfil` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`descricao` VARCHAR(50) NOT NULL,
	PRIMARY KEY(`id_perfil`)
);


CREATE TABLE IF NOT EXISTS `auditoria_cancelamento` (
	`id_auditoria` INTEGER UNSIGNED NOT NULL AUTO_INCREMENT,
	`id_pedido` INT NOT NULL,
	`id_funcionario` INT NOT NULL,
	`justificativa` TEXT NOT NULL,
	`data_hora` DATETIME NOT NULL,
	PRIMARY KEY(`id_auditoria`)
);


CREATE TABLE IF NOT EXISTS `caixa` (
	`id_caixa` INTEGER NOT NULL AUTO_INCREMENT,
	`id_funcionario` INT NOT NULL COMMENT 'responsável pelo turno',
	`data_abertura` DATETIME NOT NULL,
	`data_fechamento` DATETIME,
	`valor_abertura` DECIMAL(10,2) NOT NULL,
	`valor_fechamento` DECIMAL(10,2),
	`valor_sistema` DECIMAL(10,2) COMMENT 'total apurado pelo sistema no fechamento',
	`observacao` TEXT,
	PRIMARY KEY(`id_caixa`)
) COMMENT='Abertura/fechamento de caixa por turno';


CREATE TABLE IF NOT EXISTS `caixa_movimento` (
	`id_caixa_movimento` INTEGER NOT NULL AUTO_INCREMENT,
	`tipo` VARCHAR(20) NOT NULL COMMENT 'sangria | suprimento | entrada | saida',
	`valor` DECIMAL(10,2) NOT NULL,
	`descricao` VARCHAR(255),
	`data_hora` DATETIME NOT NULL,
	`id_caixa` INT NOT NULL,
	PRIMARY KEY(`id_caixa_movimento`)
) COMMENT='Sangria, suprimento e entradas/saídas avulsas do caixa';


CREATE TABLE IF NOT EXISTS `permissao` (
	`id_permissao` INTEGER NOT NULL AUTO_INCREMENT,
	`codigo` VARCHAR(50) NOT NULL UNIQUE COMMENT 'ex: pedido.cancelar',
	`descricao` VARCHAR(150) NOT NULL,
	PRIMARY KEY(`id_permissao`)
) COMMENT='Permissões granulares do sistema';


CREATE TABLE IF NOT EXISTS `perfil_permissao` (
	`id_perfil_permissao` INTEGER NOT NULL AUTO_INCREMENT,
	`id_perfil` INT NOT NULL,
	`id_permissao` INT NOT NULL,
	PRIMARY KEY(`id_perfil_permissao`)
) COMMENT='RBAC: permissões por perfil de acesso';


CREATE TABLE IF NOT EXISTS `historico_preco` (
	`id_historico` INTEGER NOT NULL AUTO_INCREMENT,
	`id_produto` INT NOT NULL,
	`preco_anterior` DECIMAL(10,2),
	`preco_novo` DECIMAL(10,2) NOT NULL,
	`data_alteracao` DATETIME NOT NULL,
	`id_funcionario` INT,
	`id_pizza` INT COMMENT 'quando a alteração é de preço de pizza',
	PRIMARY KEY(`id_historico`)
) COMMENT='Auditoria de alteração de preço de venda';


CREATE TABLE IF NOT EXISTS `avaliacao_pedido` (
	`id_avaliacao` INTEGER NOT NULL AUTO_INCREMENT,
	`id_pedido` INT NOT NULL UNIQUE,
	`id_cliente` INT NOT NULL,
	`nota` INT NOT NULL COMMENT '1 a 5',
	`comentario` TEXT,
	`data_hora` DATETIME NOT NULL,
	PRIMARY KEY(`id_avaliacao`)
) COMMENT='Nota e comentário do cliente sobre o pedido';


ALTER TABLE `cliente_telefone`
ADD FOREIGN KEY(`id_cliente`) REFERENCES `cliente`(`id_cliente`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `endereco`
ADD FOREIGN KEY(`id_cliente`) REFERENCES `cliente`(`id_cliente`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `endereco`
ADD FOREIGN KEY(`id_bairro`) REFERENCES `bairro`(`id_bairro`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `bairro`
ADD FOREIGN KEY(`id_zona`) REFERENCES `zona_entrega`(`id_zona_entrega`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `produto`
ADD FOREIGN KEY(`id_categoria`) REFERENCES `categoria_produto`(`id_categoria_produto`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_tamanho`
ADD FOREIGN KEY(`id_pizza`) REFERENCES `pizza`(`id_pizza`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_tamanho`
ADD FOREIGN KEY(`id_tamanho`) REFERENCES `tamanho_pizza`(`id_tamanho_pizza`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_sabor`
ADD FOREIGN KEY(`id_sabor`) REFERENCES `sabor_pizza`(`id_sabor_pizza`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_sabor`
ADD FOREIGN KEY(`id_pizza`) REFERENCES `pizza`(`id_pizza`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `promocao_produto`
ADD FOREIGN KEY(`id_produto`) REFERENCES `produto`(`id_produto`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `promocao_produto`
ADD FOREIGN KEY(`id_promocao`) REFERENCES `promocao`(`id_promocao`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `produto_ingrediente`
ADD FOREIGN KEY(`id_ingrediente`) REFERENCES `ingrediente`(`id_ingrediente`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `produto_ingrediente`
ADD FOREIGN KEY(`id_produto`) REFERENCES `produto`(`id_produto`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `estoque`
ADD FOREIGN KEY(`id_ingrediente`) REFERENCES `ingrediente`(`id_ingrediente`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `movimentacao_estoque`
ADD FOREIGN KEY(`id_estoque`) REFERENCES `estoque`(`id_estoque`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `movimentacao_estoque`
ADD FOREIGN KEY(`id_compra`) REFERENCES `compra`(`id_compra`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `compra`
ADD FOREIGN KEY(`id_fornecedor`) REFERENCES `fornecedor`(`id_fornecedor`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `item_compra`
ADD FOREIGN KEY(`id_compra`) REFERENCES `compra`(`id_compra`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `item_compra`
ADD FOREIGN KEY(`id_ingrediente`) REFERENCES `ingrediente`(`id_ingrediente`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pedido`
ADD FOREIGN KEY(`id_cliente`) REFERENCES `cliente`(`id_cliente`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pedido`
ADD FOREIGN KEY(`id_mesa`) REFERENCES `mesa`(`id_mesa`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pedido`
ADD FOREIGN KEY(`id_promocao`) REFERENCES `promocao`(`id_promocao`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `historico_status`
ADD FOREIGN KEY(`id_pedido`) REFERENCES `pedido`(`id_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `historico_status`
ADD FOREIGN KEY(`id_status`) REFERENCES `status_pedido`(`id_status`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `item_pedido`
ADD FOREIGN KEY(`id_pedido`) REFERENCES `pedido`(`id_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `item_pedido`
ADD FOREIGN KEY(`id_produto`) REFERENCES `produto`(`id_produto`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `item_pedido`
ADD FOREIGN KEY(`id_pizza`) REFERENCES `pizza`(`id_pizza`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_pedido`
ADD FOREIGN KEY(`id_item_pedido`) REFERENCES `item_pedido`(`id_item_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_pedido`
ADD FOREIGN KEY(`id_tamanho`) REFERENCES `tamanho_pizza`(`id_tamanho_pizza`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_pedido`
ADD FOREIGN KEY(`id_borda`) REFERENCES `borda`(`id_borda`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_pedido_sabor`
ADD FOREIGN KEY(`id_sabor`) REFERENCES `sabor_pizza`(`id_sabor_pizza`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pizza_pedido_sabor`
ADD FOREIGN KEY(`id_pizza_pedido`) REFERENCES `pizza_pedido`(`id_pizza_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `item_pedido_adicional`
ADD FOREIGN KEY(`id_item_pedido`) REFERENCES `item_pedido`(`id_item_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `item_pedido_adicional`
ADD FOREIGN KEY(`id_adicional`) REFERENCES `adicional`(`id_adicional`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `observacao_pedido`
ADD FOREIGN KEY(`id_pedido`) REFERENCES `pedido`(`id_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `entrega`
ADD FOREIGN KEY(`id_pedido`) REFERENCES `pedido`(`id_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `entrega`
ADD FOREIGN KEY(`id_motoboy`) REFERENCES `motoboy`(`id_motoboy`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `escala_motoboy`
ADD FOREIGN KEY(`id_motoboy`) REFERENCES `motoboy`(`id_motoboy`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `rota_entrega`
ADD FOREIGN KEY(`id_entrega`) REFERENCES `entrega`(`id_entrega`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `comissao_motoboy`
ADD FOREIGN KEY(`id_motoboy`) REFERENCES `motoboy`(`id_motoboy`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pagamento`
ADD FOREIGN KEY(`id_pedido`) REFERENCES `pedido`(`id_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pagamento`
ADD FOREIGN KEY(`id_forma_pagamento`) REFERENCES `forma_pagamento`(`id_forma_pagamento`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pagamento_cartao`
ADD FOREIGN KEY(`id_pagamento`) REFERENCES `pagamento`(`id_pagamento`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pagamento_cartao`
ADD FOREIGN KEY(`id_bandeira`) REFERENCES `bandeira`(`id bandeira`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pagamento_pix`
ADD FOREIGN KEY(`id_pagamento`) REFERENCES `pagamento`(`id_pagamento`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `troco`
ADD FOREIGN KEY(`id_pagamento`) REFERENCES `pagamento`(`id_pagamento`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `funcionario`
ADD FOREIGN KEY(`id_cargo`) REFERENCES `cargo`(`id_cargo`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `usuario_sistema`
ADD FOREIGN KEY(`id_funcionario`) REFERENCES `funcionario`(`id_funcionario`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `usuario_sistema`
ADD FOREIGN KEY(`id_perfil`) REFERENCES `perfil_acesso`(`id_perfil`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `auditoria_cancelamento`
ADD FOREIGN KEY(`id_pedido`) REFERENCES `pedido`(`id_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `auditoria_cancelamento`
ADD FOREIGN KEY(`id_funcionario`) REFERENCES `funcionario`(`id_funcionario`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pedido`
ADD FOREIGN KEY(`id_endereco`) REFERENCES `endereco`(`id_endereco`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pedido`
ADD FOREIGN KEY(`id_funcionario`) REFERENCES `funcionario`(`id_funcionario`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `entrega`
ADD FOREIGN KEY(`id_endereco`) REFERENCES `endereco`(`id_endereco`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `caixa`
ADD FOREIGN KEY(`id_funcionario`) REFERENCES `funcionario`(`id_funcionario`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `perfil_permissao`
ADD FOREIGN KEY(`id_perfil`) REFERENCES `perfil_acesso`(`id_perfil`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `historico_preco`
ADD FOREIGN KEY(`id_produto`) REFERENCES `produto`(`id_produto`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `historico_preco`
ADD FOREIGN KEY(`id_funcionario`) REFERENCES `funcionario`(`id_funcionario`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `avaliacao_pedido`
ADD FOREIGN KEY(`id_pedido`) REFERENCES `pedido`(`id_pedido`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `avaliacao_pedido`
ADD FOREIGN KEY(`id_cliente`) REFERENCES `cliente`(`id_cliente`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `caixa_movimento`
ADD FOREIGN KEY(`id_caixa`) REFERENCES `caixa`(`id_caixa`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `perfil_permissao`
ADD FOREIGN KEY(`id_permissao`) REFERENCES `permissao`(`id_permissao`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `pagamento`
ADD FOREIGN KEY(`id_caixa`) REFERENCES `caixa`(`id_caixa`)
ON UPDATE NO ACTION ON DELETE NO ACTION;
ALTER TABLE `historico_preco`
ADD FOREIGN KEY(`id_pizza`) REFERENCES `pizza`(`id_pizza`)
ON UPDATE NO ACTION ON DELETE NO ACTION;

select * from clientes_telefoneadicional
