# bd77

create database dbdistribuidora01;
use dbdistribuidora01;

create table tbCliente(
IdCli int primary key auto_increment,
NomeCli varchar(200) not null,
NumEnd numeric(6) not null,
CompEnd varchar(50),
CepCli numeric(8) not null
);

create table tbClientePF(
CPF numeric(11) primary key,
RG numeric(9) not null,
RG_Dig char(1) not null,
Nasc date not null,
IdCli int unique not null
);

create table tbClientePJ(
CNPJ numeric(14) primary key,
IE numeric(11) unique,
IdCli int unique not null
);

create table tbProduto(
CodigoBarras numeric(14) primary key,
NomeProd varchar(200) not null,
Valor decimal(5,2) not null,
Qtd int
);

create table tbCompra(
CodigoCompra numeric(10) primary key,
DataCompra datetime,
ValorTotal decimal(6,2) not null,
QtdTotal int not null,
NotaFiscal int,
IdCli int
);

create table tbNotaFiscal(
NotaFiscal int primary key,
TotalNota decimal(5,2) not null,
DataEmissao date not null
);

create table tbItemCompra(
NotaFiscal int,
CodigoBarras numeric(14),
ValorItem decimal(5,2) not null,
Qtd int not null,
primary key(NotaFiscal,CodigoBarras)
);

create table tbFornecedor(
IdFornecedor int auto_increment primary key,
CNPJ numeric(13) not null unique,
NomeFornecedor varchar(100) not null,
telefone numeric(11)
);

create table tbPedido(
NotaFiscalPedido int primary key,
DataCompra date not null,
ValorTotal decimal (6,2) not null,
QtdTotal int not null,
IdFornecedor int
);

create table tbPedidoProduto(
NotaFiscalPedido int,
CodigoBarras numeric(14),
primary key(NotaFiscalPedido,CodigoBarras)
);

create table tbEndereco(
CEP numeric(8) primary key,
Logradouro varchar(200),
IdBairro int not null,
IdCidade int not null,
IdUF int not null
);

create table tbBairro(
IdBairro int primary key auto_increment,
Bairro varchar(200) not null
);

create table tbCidade(
IdCidade int primary key auto_increment,
Cidade varchar(200) not null
);

create table tbUF(
IdUF int primary key auto_increment,
UF varchar(200) not null
);

alter table tbCliente add foreign key (CepCli) references tbEndereco(CEP);
alter table tbClientePF add foreign key (IdCli) references tbCliente(IdCli);
alter table tbClientePJ add foreign key (IdCli) references tbCliente(IdCli);
alter table tbCompra add foreign key (NotaFiscal) references tbNotaFiscal(NotaFiscal);
alter table tbCompra add foreign key (IdCli) references tbCliente(IdCli);
alter table tbItemCompra add foreign key (NotaFiscal) references tbCompra(NotaFiscal);
alter table tbItemCompra add foreign key (CodigoBarras) references tbProduto(CodigoBarras);
alter table tbPedido add foreign key (IdFornecedor) references tbFornecedor(IdFornecedor);
alter table tbPedidoProduto add foreign key (NotaFiscalPedido) references tbPedido(NotaFiscalPedido);
alter table tbPedidoProduto add foreign key (CodigoBarras) references tbProduto(CodigoBarras);
alter table tbEndereco add foreign key (IdBairro) references tbBairro(IdBairro);
alter table tbEndereco add foreign key (IdCidade) references tbCidade(IdCidade);
alter table tbEndereco add foreign key (IdUF) references tbUF(IdUF);

-- 1) FORNECEDOR dados
delimiter $$
create procedure sp_Insert_Fornecedor(vCnpj numeric(13), vNome varchar(100), vTelefone numeric(11))
begin
insert into tbFornecedor(CNPJ, NomeFornecedor, telefone)
values (vCnpj, vNome, vTelefone);
end $$

call sp_Insert_Fornecedor("1245678937123", "Revenda Chico Loco", "11934567897");
call sp_Insert_Fornecedor("1345678937123", "José Faz Tudo S/A", "11934567898");
call sp_Insert_Fornecedor("1445678937123", "Valdalto Entregas", "11934567899");
call sp_Insert_Fornecedor("1545678937123", "Astrogildo Das Estrelas", "11934567800");
call sp_Insert_Fornecedor("1645678937123", "Amoroso e Doce", "11934567801");
call sp_Insert_Fornecedor("1745678937123", "Marcelo Dedal", "11934567802");
call sp_Insert_Fornecedor("1845678937123", "Fransciscano Cachaça", "11934567803");
call sp_Insert_Fornecedor("1945678937123", "Joãozinho Chupeta", "11934567804");


-- 2) CIDADE dados
delimiter $$
create procedure sp_Insert_Cidade(vNomeCid varchar(200))
begin
insert into tbCidade(Cidade)
values (vNomeCid);
end$$

call sp_Insert_Cidade ("Rio de Janeiro");
call sp_Insert_Cidade ("São Carlos");
call sp_Insert_Cidade ("Campinas");
call sp_Insert_Cidade ("Franco da Rocha");
call sp_Insert_Cidade ("Osasco");
call sp_Insert_Cidade ("Ponta Grossa");


-- 3) ESTADOS dados
delimiter $$
create procedure sp_Insert_UF(vUF char(2))
begin
insert into tbUF(Uf)
values(vUF);
end $$

call sp_Insert_UF("SP");
call sp_Insert_UF ("RJ");
call sp_Insert_UF ("RS");

-- 4) BAIRRO dados
delimiter $$
create procedure sp_Insert_Bairro(vNomeBarr varchar(200))

begin
insert into tbBairro(Bairro)
values(vNomeBarr);
end$$


call sp_Insert_Bairro ("Aclimação");
call sp_Insert_Bairro ("Capão Redondo");
call sp_Insert_Bairro ("Pirituba");
call sp_Insert_Bairro ("Lapa");
call sp_Insert_Bairro ("Liberdade");


-- 5) PRODUTO dados
delimiter $$
create procedure sp_Insert_Produto(vCodigo bigint, vNomeProd varchar(200), vValor decimal(5,2), vQtd int)

begin
insert into tbProduto(CodigoBarras, NomeProd, Valor, Qtd)
values(vCodigo, vNomeProd, vValor, vQtd);
end$$

call sp_Insert_Produto(12345678910111, "Rei de Papel Mache", 54.61, 120);
call sp_Insert_Produto(12345678910112, "Bolinha de Sabão", 100.45, 120);
call sp_Insert_Produto(12345678910113, "Carro Bate Bate", 44.00, 120);
call sp_Insert_Produto(12345678910114, "Bola Furada", 10.00, 120);
call sp_Insert_Produto(12345678910115, "Maçã Laranja", 99.44, 120);
call sp_Insert_Produto(12345678910116, "Boneco do Hitler", 124.00, 200);
call sp_Insert_Produto(12345678910117, "Farinha de Surui", 50.00, 200);
call sp_Insert_Produto(12345678910118, "Zelador de Cemitério", 24.50, 100);


-- 6) ENDEREÇO dados
delimiter $$
create procedure spInsertEndereco(vCEP varchar(8), vLogradouro varchar(200), vBairro varchar(200), vCidade varchar(200), vUF char(2))
begin

declare idBairroSP int;
declare idCidadeSP int;
declare idUFSP int;
declare mensgErro text;

if not exists (select * from tbBairro where Bairro = vBairro) then
    insert into tbbairro (bairro) values (vBairro);
end if;

if not exists (select * from tbCidade where Cidade = vCidade) then
    insert into tbcidade (cidade) values (vCidade);
end if;

if not exists (select * from tbUF where  UF = vUF) then
    insert into tbuf (uf) values (vUF);
end if;

if not exists (select * from tbEndereco where vCEP = CEP) then

    set @idBairroSP = (select idBairro from tbBairro where Bairro = vBairro);
    set @idCidadeSP = (select idCidade from tbCidade where Cidade = vCidade);
    set @idUFSP = (select idUF from tbUF where UF = vUF);

        else
    select 'Já existe';
end if;
insert into tbEndereco(CEP, Logradouro, IdBairro, IdCidade, IdUF) values (vCEP, vLogradouro, @idBairroSP, @idCidadeSP, @idUFSP);
end $$

call spInsertEndereco(12345050, "Rua da Federal", "Lapa", "São Paulo", "SP");
call spInsertEndereco(12345051, "Av Brasil", "Lapa", "Campinas", "SP");
call spInsertEndereco(12345052, "Rua Liberdade", "Consolação", "São Paulo", "SP");
call spInsertEndereco(12345053, "Av Paulista", "Penha", "Rio de Janeiro", "RJ");
call spInsertEndereco(12345054, "Rua Ximbú", "Penha", "Rio de Janeiro", "RJ");
call spInsertEndereco(12345055, "Rua Piu X1", "Penha", "Campinas", "SP");
call spInsertEndereco(12345056, "Rua chocolate", "Aclimação", "Barra Mansa", "RJ");
call spInsertEndereco(12345057, "Rua Pão na Chapa", "Barra Funda", "Ponta Grossa", "RS");

select * from tbEndereco;
select * from tbBairro;
select * from tbCidade;
select * from tbUF;


show tables;
select * from tbclientepj;

-- 7) ClientePF dados
delimiter $$
   create procedure spInsertClientePF(vNomeCli varchar(200), vNumEnd  numeric(6), vCompEnd varchar(50), vCepCli numeric(8), vCPF numeric(11), vRG  numeric(9), vRG_Dig char(1), vNasc date)
   begin
    if not exists(select * from tbClientePF where CPF = vCPF) then
        insert into tbCliente(NomeCli, NumEnd, CompEnd, CepCli) values (vNomeCli, vNumEnd, vCompEnd, vCepCli);
        set @codCli = (select max(IdCli) from tbCliente);
        insert into tbClientePF(CPF, RG, RG_Dig, Nasc, IdCli) values (vCPF, vRG, vRG_Dig, vNasc, @codCli);
    end if;
end $$

SET foreign_key_checks = 0; -- Define a checagem da chave estrangeira como zero (Booleana), ent não checa a constraint dela

call spInsertClientePF("Pimpão",325,null,12345051,12345678911,12345678,"0","2000/10/12");
call spInsertClientePF("Disney Chaplin",89,"Ap. 12",12345053,12345678912,12345679,"0","2000/11/21");
call spInsertClientePF("Marciano",744,null,12345054,12345678913,12345680,"0","2000/06/01");
call spInsertClientePF("Lança Perfume",128,null,12345059,12345678914,12345681,"X","2000/04/05");
call spInsertClientePF("Remédio Amargo",2585,null,12345058,12345678915,12345682,"0","2000/07/15");
select * from tbCliente;
select * from tbClientePF;


show tables;
select * from tbcliente;
select * from tbClientePj;

describe tbcliente;
delimiter $$
 create procedure spInsertClientePJ(vNomeCli varchar(200), vNumEnd decimal(6,0), vCompEnd varchar(50), vCepCli decimal(8,0),vCNPJ decimal(14,0),vIE decimal(11,0)) 
 begin
	if not exists(select * from tbclientepj where CNPJ = vCNPJ) then
		insert into tbcliente(NomeCli, NumEnd, CompEnd, CepCli) values (vNomeCli, vNumEnd, vCompEnd, vCepCli);
		set @idCli = (select max(IdCli) from tbcliente);
		insert into tbClientePJ(CNPJ, IE, idCli) values (vCNPJ, vIE,@IdCli);
    end if;
 end
 $$
 
 call spInsertClientePJ("Pagamada", 159, null, 12345051, 12345678912345, 98765432198);
 call spInsertClientePJ("Caloxando", 69, null, 12345053, 12345678912346, 98765432199);
 call spInsertClientePJ("Semgrana", 189, null, 12345060, 12345678912347, 98765432100);
 call spInsertClientePJ("Cemreais", 5024, "Sala 23", 12345060, 12345678912348, 98765432101);
 call spInsertClientePJ("Durango", 1254, null, 12345060, 12345678912349, 98765432102);
 select * from tbcliente;
 select * from tbClientePf;

