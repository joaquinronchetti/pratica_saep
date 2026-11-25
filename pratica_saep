-- create table jogadores (
--     id_jogador serial primary key,
--     nickname varchar(50) not null unique,
--     email varchar(100) not null unique,
--     senha varchar(50) not null,
--     ativo boolean default true
-- );

-- create table modalidades (
--     id_modalidade serial primary key,
--     nome varchar(50) not null unique,
--     categoria varchar(50) not null
-- );

-- create table torneios (
--     id_torneio serial primary key,
--     nome varchar(100) not null,
--     id_modalidade integer references modalidades(id_modalidade),
--     data_inicio date not null,
--     data_fim date not null,
--     check (data_fim >= data_inicio)
-- );

-- create table partidas (
--     id_partida serial primary key,
--     titulo varchar(100) not null,
--     id_jogador integer not null references jogadores(id_jogador),
--     id_torneio integer not null references torneios(id_torneio),
--     data_agendamento timestamp not null,
--     data_criacao timestamp,
--     status varchar(30) default 'Agendada',
--     check (status in ('Agendada', 'Em andamento', 'Finalizada', 'Cancelada'))
-- );

-- create table historico_partidas (
--     id_historico serial primary key,
--     id_partida integer not null references partidas(id_partida),
--     descricao varchar(200) not null,
--     data_registro timestamp default current_timestamp
-- );

-- insert into jogadores (nickname, email, senha, ativo) values
-- ('joao gamer', 'joao@gmail.com', '123456', true),
-- ('enzox', 'enzox@gmail.com', '123456', true),
-- ('proplayer1', 'proplayer1@gmail.com', '123456', false);

-- insert into modalidades (nome, categoria) values
-- ('valorant', 'fps'),
-- ('league of legends', 'moba'),
-- ('counter strike 2', 'fps');

-- insert into torneios (nome, id_modalidade, data_inicio, data_fim) values
-- ('copa valorant', 1, '2026-12-01', '2026-12-10'),
-- ('liga lol', 2, '2026-12-05', '2026-12-15'),
-- ('torneio cs2', 3, '2026-12-10', '2026-12-20');

-- create or replace function fn_validar_partida()
-- returns trigger as $$
-- begin
--     if not exists (
--         select 1 from jogadores
--         where id_jogador = new.id_jogador
--         and ativo = true
--     ) then
--         raise exception 'jogador nao esta ativo';
--     end if;

--     new.data_criacao = current_timestamp;

--     return new;
-- end;
-- $$ language plpgsql;

-- create trigger trg_validar_partida
-- before insert on partidas
-- for each row
-- execute function fn_validar_partida();

-- create or replace function fn_historico()
-- returns trigger as $$
-- begin
--     insert into historico_partidas (id_partida, descricao)
--     values (new.id_partida, 'partida criada: ' || new.titulo);

--     return new;
-- end;
-- $$ language plpgsql;

-- create trigger trg_historico
-- after insert on partidas
-- for each row
-- execute function fn_historico();

-- insert into partidas
-- (titulo, id_jogador, id_torneio, data_agendamento, status) values
-- ('joao gamer vs enzox', 1, 1, '2026-12-02 15:00', 'Agendada'),
-- ('enzox vs joao gamer', 2, 2, '2026-12-06 16:00', 'Agendada'),
-- ('final do cs2', 1, 3, '2026-12-12 18:00', 'Em andamento');

-- create or replace view vw_painel_principal as
-- select
--     j.nickname,
--     p.titulo,
--     m.categoria,
--     p.data_agendamento,
--     p.status
-- from partidas p
-- join jogadores j on p.id_jogador = j.id_jogador
-- join torneios t on p.id_torneio = t.id_torneio
-- join modalidades m on t.id_modalidade = m.id_modalidade;

-- create or replace function fn_buscar_jogadores(termo text)
-- returns table (
--     id_jogador integer,
--     nickname varchar,
--     email varchar
-- )
-- as $$
-- begin
--     return query
--     select j.id_jogador, j.nickname, j.email
--     from jogadores j
--     where j.nickname ilike '%' || termo || '%'
--     or j.email ilike '%' || termo || '%';
-- end;
-- $$ language plpgsql;

-- create or replace view vw_cronograma_partidas as
-- select
--     p.id_partida,
--     p.titulo,
--     j.nickname,
--     p.data_agendamento,
--     p.status,
--     p.data_agendamento::date - current_date as dias_restantes
-- from partidas p
-- join jogadores j on p.id_jogador = j.id_jogador
-- order by p.data_agendamento;
