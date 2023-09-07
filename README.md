# Banco-de-Dados
Projeto Banco de Dados
Segue o código.


CREATE DATABASE Loja_de_Roupa

CREATE TABLE Marca (
CodigoMarca smallint identity PRIMARY KEY,
DescricaoMarca varchar(30) not null,
DataCadastro datetime
)

CREATE TABLE Categoria (
CodigoCategoria tinyint identity PRIMARY KEY,
DescricaoCategoria varchar(30) not null,
DataCadastro datetime
)
CREATE TABLE Cargo (
CodigoCargo tinyint identity PRIMARY KEY,
DescricaoCargo varchar(40) unique not null,
DataCadastro datetime
)

CREATE TABLE Fornecedor (
CodigoFornecedor smallint identity PRIMARY KEY,
NomeFornecedor varchar(60) not null,
CNPJ char(18) not null unique, --00.000.000/0000-00
IE varchar(15), --000.000.000.000
Rua varchar(50) not null,
Numero smallint not null,
Complemento varchar(30),
Bairro varchar(40) not null,
CEP char(9) not null, --00000-000
Cidade varchar(30) not null,
Estado char(2) not null Constraint DefaultEstadoFornecedor default 'SP',
Telefone01 char(13) not null, --(00)0000-0000
Telefone02 char(13), --(00)0000-0000
Celular01 char(14), --(00)00000-0000
Celular02 char(14), --(00)00000-0000
EmailFornecedor varchar(60) not null,
NomeContato varchar(50),
ObservacaoFornecedor varchar(500),
StatusFornecedor bit, --0 = Inativo / 1 = Ativo
DataCadastro datetime
)

CREATE TABLE Cliente (
CodigoCliente int identity PRIMARY KEY,
NomeCliente varchar(50) not null,
SexoCliente char(1) not null,
DataNascimentoCliente date,
RGCliente varchar(12), --00.000.000-0
CPFCliente char(14) unique not null, --000.000.000-00
EstadoCivilCliente varchar(20),
Rua varchar(50) not null,
Numero smallint not null,
Complemento varchar(30),
Bairro varchar(40) not null,
CEP char(9) not null,-- 00000-000
Cidade varchar(30) not null,
Estado char(2) not null Constraint DefaultEstadoCliente default 'SP',
Telefone01 char(13), --(00)0000-0000
Telefone02 char(13), --(00)0000-0000
Celular01 char(14), --(00)00000-0000
Celular02 char(14), --(00)00000-0000
EmailCliente varchar(50),
ObservacaoCliente varchar(500),
StatusCliente bit, --0 = Inativo / 1 = Ativo
DataCadastro datetime
)

CREATE TABLE Funcionario (
CodigoFuncionario smallint identity PRIMARY KEY,
NomeFuncionario varchar(50) not null,
SexoFuncionario char(1) not null,
DataNascimentoFuncionario date not null,
RGFuncionario varchar(12), --00.000.000-0
CPFFuncionario char(14) unique not null, --000.000.000-00
EstadoCivilFuncionario varchar(20),
DataAdmissao date not null,
DataDemissao date,
Salario decimal(7,2) not null, --10000,00
Rua varchar(50) not null,
Numero smallint not null,
Complemento varchar(30),
Bairro varchar(40) not null,
CEP char(9) not null, --00000-000
Cidade varchar(30) not null,
Estado char(2) not null Constraint DefaultEstadoFuncionario default 'SP',
Telefone01 char(13), --(00)0000-0000
Telefone02 char(13), --(00)0000-0000
Celular01 char(14), --(00)00000-0000
Celular02 char(14), --(00)00000-0000
EmailFuncionario varchar(50),
ObservacaoFuncionario varchar(500),
StatusFuncionario bit, --0 = Inativo / 1 = Ativo
DataCadastro datetime,
CodigoCargo tinyint foreign key references Cargo not null
)

CREATE TABLE Produto (
CodigoProduto int identity PRIMARY KEY,
DescricaoProduto varchar(30) not null,
PrecoCompra decimal(7,2) not null, --10000,00
PrecoVenda decimal(7,2) not null, --10000,00
QuantidadeEstoque smallint not null,ObservacaoProduto varchar(500),
StatusProduto bit, --0 = Inativo / 1 = Ativo
DataCadastro datetime,
CodigoCategoria tinyint foreign key references Categoria not null,
CodigoMarca smallint foreign key references Marca not null
)

CREATE TABLE ProdForn (
CodigoProdForn int identity PRIMARY KEY,
DataCompra date not null,
NumNF int not null unique,
CodigoProduto int foreign key references Produto not null,
CodigoFornecedor smallint foreign key references Fornecedor not null
)

CREATE TABLE Venda (
CodigoVenda int identity PRIMARY KEY,
Desconto tinyint,
Acrescimo tinyint,
ValorTotal smallmoney not null,
ObservacaoVenda varchar(500),
DataVenda datetime,
CodigoFuncionario smallint foreign key references Funcionario not null,
CodigoCliente int foreign key references Cliente not null
)

CREATE TABLE ItensVenda (
CodigoItensVenda int identity PRIMARY KEY,
ValorUnitario decimal(7,2), --100000,00
QuantidadeProduto tinyint,
CodigoProduto int foreign key references Produto not null,
CodigoVenda int foreign key references Venda not null
)
--CARGO
insert into Cargo values ('Gerente',convert(date, getdate(), 103));
insert into Cargo values ('Vendedor',convert(date, getdate(), 103));
insert into Cargo values ('Caixa',convert(date, getdate(), 103));
insert into Cargo values ('Estoquista',convert(date, getdate(), 103));
insert into Cargo values ('Auxiliar de Administração',convert(date, getdate(), 103));
--CLIENTE
insert into Cliente values ('Flávio de Souza', 'M', convert(date, '10/05/1984', 103),
'48.987.570-X', '574.548.984-10', 'Casado', 'Av. Raposo Tavares', '547', '', 'Jardim 
Glória', '13401-745', 'Piracicaba', 'SP', '(19)3452-7710', '', '(19)99641-0057', '',
'flavio.souza@hotmail.com', '', 1, getdate());
insert into Cliente values ('Samanta Oliveira', 'F', convert(date, '02/12/1990', 103),
'48.487.070-4', '444.528.264-20', 'Solteiro (a)', 'Av. Dr. Paulo de Moraes', '555',
'', 'Paulista', '13401-022', 'Piracicaba', 'SP', '(19)3444-2216', '', '', '', '', '',
1, getdate());
insert into Cliente values ('Carlos José da Silva', 'M', convert(date, '30/08/1980',
103), '28.666.874-2', '222.488.190-00', 'Separado (a)', 'Rua Albert Einstei', '77',
'Apartamento 2', 'Jardim Ibirapuera', '13341-715', 'São Paulo', 'SP', '(11)3452-7710',

'(11)2546-8741', '(19)99641-0057', '(19)99147-2221', 'carlos@yahoo.com.br', 'cliente 
deve R$50,00', 1, getdate());
insert into Cliente values ('Amanda Aparecida', 'F', convert(date, '25/01/1995', 103),
'', '474.568.111-60', 'Solteiro (a)', 'Av. Marquês de Monte Alegre', '111', '',
'Pauliceia', '13401-476', 'Piracicaba', 'SP', '', '', '(19)99117-0057', '', '', '', 1,
getdate());
insert into Cliente values ('Fabiola Castro', 'F', convert(date, '11/05/1984', 103),
'', '222.548.984-10', 'Separado (a)', 'Av. Raposo Tavares', '887', '', 'Jardim 
Glória', '13401-745', 'Piracicaba', 'SP', '', '', '(19)99175-1558', '',
'fabiola.c@hotmail.com', '', 1, getdate());
--FUNCIONARIO
insert into Funcionario values ('Pedro Souza', 'M', convert(date, '01/03/1993', 103),
'45.227.874-5', '786.218.576-51', 'Solteiro (a)', convert(date, '10/02/2015 00:00:00',
103), null, 889.54, 'Av. do Café', '487', 'Apartamento 1', 'Paulista', '13401-090',
'Piracicaba', 'SP', '(19)3457-9876', '(19)3487-8465', '(19)99648-5721', '(19)99154-
8919', 'pedro.souza@gmail.com', 'Funcionário em treinamento', 1, getdate(), 2);
insert into Funcionario values ('Aline Barros', 'F', convert(date, '07/05/1991', 103),
'45.277.774-4', '786.257.571-00', 'Casado', convert(date, '15/07/2010 00:00:00', 103),
null, 1509.63, 'Av. do Café', '33', '', 'Paulista', '13401-090', 'Piracicaba', 'SP',
'(19)3422-9656', '', '(19)99644-5001', '', 'aline@hotmail.com', '', 1, getdate(), 1);
insert into Funcionario values ('Samira Pereira', 'F', convert(date, '08/03/1985',
103), '77.247.774-X', '222.218.476-51', 'Casado', convert(date, '01/10/2012 00:00:00',
103), null, 1050.14, 'Av. Morais Barros', '1025', '', 'Centro', '13411-450',
'Piracicaba', 'SP', '', '', '(19)99655-5004', '', '', '', 1, getdate(), 3);
insert into Funcionario values ('Paulo Camargo', 'M', convert(date, '15/12/1985',
103), '22.227.564-5', '787.048.666-00', 'Separado (a)', convert(date, '21/02/2014 
00:00:00', 103), null, 782.30, 'Rua Floriano Peixoto', '2221', '', 'Centro', '13411-
440', 'Piracicaba', 'SP', '(19)2557-3669', '', '(19)99144-5631', '',
'paulinho@gmail.com', '', 1, getdate(), 4);
insert into Funcionario values ('Samanta Bortoleto', 'F', convert(date, '01/03/1990',
103), '45.117.694-X', '226.668.571-50', 'Casado', convert(date, '05/01/2013 00:00:00',
103), null, 1190.05, 'Rua Benjamin Constant', '1225', '', 'Centro', '13441-113',
'Piracicaba', 'SP', '', '', '(19)97555-5000', '', '', '', 1, getdate(), 5);
--FORNECEDOR
insert into Fornecedor values ('Veste Tudo Distribuidora LTDA', '65.432.121/6546-
54','545.465.432.132', 'Av. Dr. Paulo de Morais', '521', '', 'Paulista', '13443-215',
'Piracicaba', 'SP', '(19)3456-4942', '(19)2546-8321', '(19)99654-5454', '(19)99654-
5454', 'contato@vestetudo.com.br','Paula', 'Fornecedor atrasou várias entregas', 1,
getdate());
insert into Fornecedor values ('Boa Compra Roupas e Acessórios LTDA',
'41.432.121/0001-54','225.463.432.002', 'Av. Piracicamirim', '774', '', 'Vila 
Monteiro', '14343-115', 'Piracicaba', 'SP', '(19)2551-4412', '', '', '',
'contato@boacompra.com.br','Débora', '', 1, getdate());
insert into Fornecedor values ('Super Roupas LTDA ME', '11.400.121/0001-05','', 'Av. 
Rio das Pedras', '117', '', 'Jardim Aricanduva', '11400-223', 'São Paulo', 'SP',
'(11)2556-2222', '', '(11)97534-5444', '', 'contato@superroupas.com.br','Eitor', '',
1, getdate());
insert into Fornecedor values ('Rei dos Jeans LTDA EPP', '44.672.121/0001-01','', 'Av.
Rio das Pedras', '1123', '', 'Jardim Aricanduva', '11400-223', 'São Paulo', 'SP',
'(11)3366-4942', '', '', '', 'contato@reidosjeans.com.br','Bárbara', '', 1,
getdate());
insert into Fornecedor values ('Mamãe Coruja Roupas Infantis LTDA', '44.112.133/0002-
02','445.111.439.102', 'Rua Dona Jane Conceição', '440', '', 'Paulista', '13000-111',
'Piracicaba', 'SP', '(19)2536-1112', '', '(19)99100-5224', '',
'contato@mamaecoruja.com.br','Rodrigo', '', 1, getdate());
--CATEGORIA
insert into Categoria values ('Calça',getdate());
insert into Categoria values ('Jaqueta',getdate());
insert into Categoria values ('Vestido',getdate());
insert into Categoria values ('Camiseta',getdate());
insert into Categoria values ('Meia',getdate());
--MARCA
insert into Marca values ('Kanui',getdate());
insert into Marca values ('Pernambucanas',getdate());
insert into Marca values ('Hot Point',getdate());
insert into Marca values ('Monster',getdate());
insert into Marca values ('Quarta Estação',getdate());
--PRODUTO
insert into Produto values ('Calça Jeans', 29.95, 89.90, 10, 'Promoção até 
25/06/2015', 1, getdate(), 4, 4);
insert into Produto values ('Camiseta Infantil', 10.00, 23.90, 15, '', 1, getdate(),
4, 5);
insert into Produto values ('Jaqueta de Couro', 32.60, 83.20, 5, '', 1, getdate(), 2,
3);
insert into Produto values ('Meia Branca', 5.20, 13.60, 40, '', 1, getdate(), 5, 2);
insert into Produto values ('Calça Legging', 12.45, 21.80, 35, '', 1, getdate(), 1,
1);
--PRODUTO/FORNECEDOR
insert into ProdForn values (convert(date, getdate(), 103),1567,1,1);
insert into ProdForn values (convert(date, getdate(), 103),1698,1,2);
insert into ProdForn values (convert(date, getdate(), 103),1677,2,3);
insert into ProdForn values (convert(date, getdate(), 103),1687,3,1);
insert into ProdForn values (convert(date, getdate(), 103),1987,3,2);
insert into ProdForn values (convert(date, getdate(), 103),1988,3,3);
insert into ProdForn values (convert(date, getdate(), 103),2001,4,5);
insert into ProdForn values (convert(date, getdate(), 103),2112,5,4);
insert into ProdForn values (convert(date, getdate(), 103),2136,5,5);
--VENDA
insert into Venda values (0, 0, 51.10, 'Primeira Parcela: 05/07 Segunda Parcela: 
05/08', getdate(), 2, 1);
insert into Venda values (5, 0, 20.71, '', getdate(), 2, 1);
insert into Venda values (0, 0, 197.00, '', getdate(), 2, 2);
insert into Venda values (0, 0, 40.80, '', getdate(), 2, 3);
insert into Venda values (0, 0, 21.80, '', getdate(), 2, 5);
--ITENS VENDA
--VENDA 01
insert into ItensVenda values (23.90, 1, 2, 1);
insert into ItensVenda values (27.20, 2, 4, 1);
--VENDA 02
insert into ItensVenda values (21.80, 1, 5, 2);
--VENDA 03
insert into ItensVenda values (89.90, 1, 1, 3);
insert into ItensVenda values (23.90, 1, 2, 3);
insert into ItensVenda values (83.20, 1, 3, 3);
--VENDA 04
insert into ItensVenda values (40.80, 3, 4, 4);
--VENDA 05
insert into ItensVenda values (21.80, 1, 5, 5);
--CARGO
update Cargo set DescricaoCargo = 'Operador de Caixa' where DescricaoCargo = 'Caixa';
update Cargo set DescricaoCargo = 'Auxiliar de Escritório' where CodigoCargo = 5;
--CLIENTE
update Cliente set DataNascimentoCliente = '10.05.1985' where NomeCliente = 'Flávio de
Souza';
update Cliente set NomeCliente = 'Samanta Oliveira da Silva', EstadoCivilCliente =
'Casado (a)' where CodigoCliente = 2;
update Cliente set Cidade = 'Campinas' where SexoCliente = 'M';
--FUNCIONARIO
update Funcionario set EstadoCivilFuncionario = 'Casado (a)' where CodigoFuncionario =
1;
update Funcionario set CodigoCargo = 3 where NomeFuncionario = 'Aline Barros';
update Funcionario set DataDemissao = convert(date, '30/04/2017', 103),
StatusFuncionario = 0 where NomeFuncionario = 'Pedro Souza';
update Funcionario set Salario = 1000.00 where Salario < 1000.00;
update Funcionario set EmailFuncionario = null where (EmailFuncionario <> '') or
(EmailFuncionario <> null);
--FORNECEDOR
update Fornecedor set NomeFornecedor = 'Veste Tudo Distrib.' where CodigoFornecedor =
1;
update Fornecedor set NomeContato = 'Camila' where NomeContato = 'Bárbara';
update Fornecedor set Celular01 = '(11)97714-5100' where CodigoFornecedor = 3;
update Fornecedor set StatusFornecedor = 0 where NomeFornecedor like '%Roupas Infantis
%';
--CATEGORIA
update Categoria set DescricaoCategoria = 'Calca' where CodigoCategoria = 1;
update Categoria set DescricaoCategoria = 'Moletom' where DescricaoCategoria = 'Meia';
--MARCA
update Marca set DescricaoMarca = 'Marisa' where CodigoMarca = 1;
update Marca set DescricaoMarca = 'Renner' where DescricaoMarca = 'Hot Point';
--PRODUTO
update Produto set PrecoVenda = 20.00 where PrecoVenda < 20.00;
update Produto set PrecoVenda = 90.00 where PrecoCompra > 30.00;
update Produto set DescricaoProduto = 'Camiseta' where DescricaoProduto like
'%Infantil%';
update Produto set DescricaoProduto = 'Vestido de Alça', CodigoCategoria = 3 where
CodigoProduto = 5;
--PRODUTO/FORNECEDOR
update ProdForn set CodigoFornecedor = 2 where CodigoFornecedor = 1;
update ProdForn set CodigoProduto = 3 where CodigoProdForn = 9;
update ProdForn set CodigoProduto = 5 where CodigoProdForn = 1;
update ProdForn set DataCompra = convert(date, '15/01/2017', 103) where
CodigoFornecedor = 4;
--VENDA
update Venda set Acrescimo = 3, ValorTotal = (ValorTotal * 1.03) where CodigoVenda =
1;
update Venda set CodigoFuncionario = 4 where CodigoVenda = 2;
update Venda set CodigoCliente = 4 where CodigoFuncionario = 4;
update Venda set ValorTotal = 20.70 where ValorTotal = 20.71;
--ITENS VENDA
update ItensVenda set ValorUnitario = 27.00 where CodigoItensVenda = 2;
update ItensVenda set QuantidadeProduto = 2 where CodigoItensVenda = 8;
update ItensVenda set CodigoProduto = 5, ValorUnitario = 12.45 where CodigoProduto =
2;
update ItensVenda set CodigoVenda = 2 where CodigoItensVenda = 6;
--CARGO
select *
from Cargo C
where C.DescricaoCargo like '%a%';
select C.DescricaoCargo
from Cargo C
where C.CodigoCargo = 5
--CLIENTE
select C.NomeCliente, C.Telefone01, C.Celular01
from Cliente C
where C.SexoCliente = 'F'
select C.CodigoCliente, C.NomeCliente, V.CodigoVenda, V.ValorTotal, IV.CodigoProduto,
P.DescricaoProduto
from Cliente C, Venda V, ItensVenda IV, Produto P
where V.CodigoVenda = IV.CodigoVenda
 and P.CodigoProduto = IV.CodigoItensVenda
 and C.CodigoCliente = V.CodigoCliente
--FUNCIONARIO
select F.NomeFuncionario, F.Salario
from Funcionario F
where F.Salario > 1500.00
select F.NomeFuncionario, count(V.CodigoVenda) as Qtde
from Funcionario F, Venda V
where F.CodigoFuncionario = V.CodigoVenda
group by F.NomeFuncionario
--FORNECEDOR
select F.NomeFornecedor, F.NomeContato, F.Rua, F.Numero, F.Bairro, F.CEP, F.Telefone01
from Fornecedor F
where F.Cidade = 'Piracicaba'
--CATEGORIA
select *
from Categoria C
where C.DescricaoCategoria like '%ta'
--MARCA
select *
from Marca M
where M.CodigoMarca in (1,3,5);
--PRODUTO
select P.DescricaoProduto, P.PrecoCompra, P.ObservacaoProduto
from Produto P
where P.ObservacaoProduto is not null
 and P.PrecoCompra >= 5.00 and P.PrecoCompra <= 10.00 
--PRODUTO/FORNECEDOR
select *
from ProdForn PF
where PF.CodigoProdForn in (1,2)
select PF.DataCompra, F.NomeFornecedor, P.DescricaoProduto
from ProdForn PF, Fornecedor F, Produto P
where F.CodigoFornecedor = PF.CodigoFornecedor
 and P.CodigoProduto = PF.CodigoProduto
 and PF.CodigoProduto = 1;
--VENDA
select F.NomeFuncionario, V.ValorTotal
from Funcionario F, Venda V
where F.CodigoFuncionario = V.CodigoFuncionario
 and V.ValorTotal = (select max(V.ValorTotal)
 from Venda V)
 
select V.CodigoVenda, C.NomeCliente, C.Celular01, P.DescricaoProduto
from Venda V, ItensVenda IV, Produto P, Cliente C
where V.CodigoVenda = IV.CodigoVenda
 and P.CodigoProduto = IV.CodigoProduto
 and C.CodigoCliente = V.CodigoCliente
 and C.NomeCliente like 'Fl%'
--ITENS VENDA
select V.CodigoVenda, P.DescricaoProduto, IV.QuantidadeProduto
from ItensVenda IV, Venda V, Produto P
where V.CodigoVenda = IV.CodigoVenda
 and P.CodigoProduto = IV.CodigoProduto
--ITENS VENDA
delete from ItensVenda where CodigoVenda = 4;
delete from ItensVenda where CodigoProduto = 5 and CodigoVenda = 1;
delete from ItensVenda where CodigoItensVenda = 5;
delete from ItensVenda;
--VENDA
delete from Venda where CodigoVenda = 4
delete from Venda where ValorTotal > 100.00;
delete from Venda where Desconto is not null and Desconto <> 0;
delete from Venda;
--PRODUTO/FORNECEDOR
delete from ProdForn where DataCompra < convert(date, '09/05/2017', 103);
delete from ProdForn where CodigoFornecedor = 2;
delete from ProdForn where ProdForn.CodigoProduto = 1 and CodigoFornecedor = 3;
delete from ProdForn;
--PRODUTO
delete from Produto where CodigoProduto = 4;
delete from Produto where DescricaoProduto like 'C%' and PrecoCompra > 10.00;
delete from Produto;
--MARCA
delete from Marca where (CodigoMarca = 1) or (CodigoMarca = 3);
delete from Marca where DescricaoMarca like 'Per%';
delete from Marca;
--CATEGORIA
delete from Categoria where DescricaoCategoria like 'C%';
delete from Categoria where CodigoCategoria = 5;
delete from Categoria;
--FORNECEDOR
delete from Fornecedor where CNPJ like '44.%';
delete from Fornecedor where CodigoFornecedor = 1;
delete from Fornecedor where NomeFornecedor like '%Coruja%';
delete from Fornecedor;
--FUNCIONARIO
delete from Funcionario where StatusFuncionario = 0;
delete from Funcionario where NomeFuncionario = 'Paulo Camargo';
delete from Funcionario where year(DataNascimentoFuncionario) < '1990';
delete from Funcionario;
--CLIENTE
delete from Cliente where SexoCliente = 'M';
delete from Cliente where ObservacaoCliente is not null;
delete from Cliente where Cidade <> 'Piracicaba';
delete from Cliente;
--CARGO
delete from Cargo where DescricaoCargo LIKE '%a%';
delete from Cargo where CodigoCargo = 1;
delete from Cargo;

