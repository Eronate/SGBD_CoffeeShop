create table client
(
id_client number(7),
nume varchar2(20) constraint client_nume not null,
prenume varchar2(20) constraint client_prenume not null,
email varchar2(20),
CONSTRAINT client_pk PRIMARY KEY (id_client),
CONSTRAINT email_unique unique (email)
);

create table comanda
(
id_comanda number(9),
id_client number(7) constraint com_id_client_null not null,
data_comanda date constraint data_com_null not null,
precizari varchar2(64),
CONSTRAINT comanda_pk PRIMARY KEY (id_comanda),
CONSTRAINT comanda_client_fk FOREIGN KEY (id_client)
references client(id_client)
);

create table cafea
(
id_cafea number(7),
nume_cafea varchar2(20) constraint ncaf_null not null,
descriere varchar2(64) constraint descrierecaf_null not null,
tip_cafea varchar2(20) constraint tipcaf_null not null,
pret number(3) constraint pret_null not null,
CONSTRAINT cafea_pk PRIMARY KEY (id_cafea)
);

create table item_comanda
(
id_comanda number(9) constraint itmcom_com_null not null,
id_cafea number(7) constraint itmcom_caf_null not null,
cantitate number(2) constraint itcmcom_cantitate not null,
CONSTRAINT item_comanda_cafea_fk FOREIGN KEY (id_cafea)
references cafea(id_cafea),
CONSTRAINT item_comanda_comanda_fk FOREIGN KEY (id_comanda)
references comanda(id_comanda)
);

create table ingredient
(
id_ingredient number(7),
nume_ingredient varchar2(20) constraint ingr_nume_null not null,
constraint ingredient_pk primary key (id_ingredient)
);

create table contine_ingrediente
(
id_cafea number(7) constraint ci_caf_null not null,
id_ingredient number(7) constraint ci_ing_null not null,
cantitate number(4) constraint ci_cant_null not null,
constraint ci_cafea_fk foreign key (id_cafea)
references cafea(id_cafea),
constraint ci_ingredient_fk foreign key (id_ingredient)
references ingredient(id_ingredient)
);

create table inventar_ingrediente
(
id_ingredient number(7),
cantitate_disponibila number(9) constraint inv_ing_cant_null not null, 
constraint inv_ing_ingredient_fk foreign key(id_ingredient)
references ingredient(id_ingredient)
);

create table review
(
id_review number(7),
id_client number(7) constraint rev_client_null not null,
descriptie varchar2(64),
nr_stele number(1) constraint nr_stele_null not null,
constraint review_pk primary key (id_review),
constraint client_review_fk foreign key (id_client) 
references client(id_client),
check (nr_stele >0),
check (nr_stele <6)
);

create table promotie
(
id_promotie number(7),
id_comanda number(9) constraint prom_com_null not null,
id_client number(7) constraint prom_client_null not null,
constraint promotie_pk primary key(id_promotie),
constraint promotie_comanda_fk foreign key(id_comanda)
references comanda(id_comanda),
constraint promotie_client_fk foreign key(id_client)
references client(id_client)
);

create table tip_promotie
(
id_promotie number(7) constraint tipprom_id_null not null,
detalii_promotie varchar2(20) constraint tipprom_detalii not null,
constraint tip_promotie_promotie_fk foreign key(id_promotie)
references promotie(id_promotie)
);

create table gen
(
id_gen number(7),
nume_gen varchar2(20) constraint numegen_null not null,
constraint id_gen_pk primary key(id_gen)
);

create table carte
(
id_carte number(7),
id_gen number(7) constraint carte_gen_null not null,
nume_carte varchar2(20) constraint carte_nume_null not null,
autor varchar2(20) constraint carte_autor_null not null,
prezentare_carte varchar2(64),
constraint carte_gen_fk foreign key (id_gen) 
references gen(id_gen),
constraint carte_pk primary key (id_carte) 
);

create table imprumut
(
id_client number(7) not null,
id_carte number(7) not null,
data_imprumut date not null,
data_restituire date,
constraint imprumut_client_fk foreign key (id_client) 
references client(id_client),
constraint imprumut_carte_fk foreign key (id_carte) 
references carte(id_carte)
);

create table inventar_carti
(
id_carte number(7) not null,
cantitate number(2) not null,
constraint ic_fk foreign key (id_carte)
references carte(id_carte)
);

alter table cafea 
modify nume_cafea varchar2(40); 
  
alter table cafea 
modify descriere varchar2(80); 

alter table tip_promotie 
modify detalii_promotie varchar2(80); 
  
alter table tip_promotie 
add tip_promotie_id number(3) not null; 
  
alter table tip_promotie 
drop column id_promotie; 
  
alter table tip_promotie 
add tip_promotie_id number(3) not null; 
  
alter table tip_promotie 
add primary key (tip_promotie_id); 
  
alter table promotie 
drop column id_promotie; 
  
alter table promotie 
add tip_promotie_id number(3) not null; 
  
alter table promotie 
add foreign key (tip_promotie_id) references tip_promotie(tip_promotie_id); 

--INSERAREA IN CAFEA
create sequence cafeaid_seq 
start with 1
increment by 1;

insert into cafea values
(cafeaid_seq.nextval, 'Espresso scurt', 'Un shot concentrat de espresso', 'Small', 7);

insert into cafea values
(cafeaid_seq.nextval, 'Espresso lung', 'Un shot de espresso preparat cu o cantitate mai mare de apa', 'Small', 7);

insert into cafea values
(cafeaid_seq.nextval, 'Caffe Latte', 'Cafea echilibrata cu lapte fierbinte si acoperita cu un strat fin de spuma', 'Grande', 8);

insert into cafea values
(cafeaid_seq.nextval, 'Vanilla Latte', 'Cafea echilibrata cu lapte fierbinte si aromata cu sirop de vanilie', 'Grande', 10);

insert into cafea values
(cafeaid_seq.nextval, 'Caffe Mocha', 'Espresso combinat cu sirop de ciocolată și lapte fierbinte', 'Grande', 10);

insert into cafea values
(cafeaid_seq.nextval, 'Gingerbread Latte', 
'Siropul de turtă dulce și siropul mocha sunt amestecate cu espresso si lapte, topping de napolitane', 'Venti', 20);

insert into cafea values
(cafeaid_seq.nextval, 'Iced Caramel Macchiato', 'Espresso cu sirop de caramel, vanilie si cuburi de gheata', 'Venti', 18);

--INSERAREA IN INGREDIENTE
create sequence ingredientid_seq 
start with 1
increment by 1;

insert into ingredient values
(ingredientid_seq.nextval, 'Cafea boabe');

insert into ingredient values
(ingredientid_seq.nextval, 'Lapte');

insert into ingredient values
(ingredientid_seq.nextval, 'Sirop de vanilie');

insert into ingredient values
(ingredientid_seq.nextval, 'Sirop de ciocolata');

insert into ingredient values
(ingredientid_seq.nextval, 'Sirop mocha');

insert into ingredient values
(ingredientid_seq.nextval, 'Sirop turta dulce');

insert into ingredient values
(ingredientid_seq.nextval, 'Sirop caramel');

insert into ingredient values
(ingredientid_seq.nextval, 'Napolitane sfaramate');

insert into ingredient values
(ingredientid_seq.nextval, 'Cuburi de gheata');

--INSERARE IN CONTINE_INGREDIENTE

insert into contine_ingrediente values
(1, 1, 10);

insert into contine_ingrediente values
(2, 1, 10);

insert into contine_ingrediente values
(3, 1, 20);

insert into contine_ingrediente values
(3, 2, 300);

insert into contine_ingrediente values
(4, 1, 20);

insert into contine_ingrediente values
(4, 2, 300);

insert into contine_ingrediente values
(4, 3, 25);

insert into contine_ingrediente values
(5, 1, 20);

insert into contine_ingrediente values
(5, 2, 300);

insert into contine_ingrediente values
(5, 4, 25);

insert into contine_ingrediente values
(6, 1, 30);

insert into contine_ingrediente values
(6, 2, 300);

insert into contine_ingrediente values
(6, 6, 25);

insert into contine_ingrediente values
(6, 5, 25);

insert into contine_ingrediente values
(6, 8, 35);

insert into contine_ingrediente values
(7, 1, 30);

insert into contine_ingrediente values
(7, 7, 25);

insert into contine_ingrediente values
(7, 3, 25);

insert into contine_ingrediente values
(7, 9, 3);

--INSERARE IN INVENTAR_INGREDIENTE

insert into inventar_ingrediente values
(1, 100000);

insert into inventar_ingrediente values
(2, 50000);

insert into inventar_ingrediente values
(3, 4000);

insert into inventar_ingrediente values
(4, 4000);

insert into inventar_ingrediente values
(5, 4000);

insert into inventar_ingrediente values
(6, 2000);

insert into inventar_ingrediente values
(7, 4000);

insert into inventar_ingrediente values
(8, 1000);

insert into inventar_ingrediente values
(9, 100);

--INSERARE IN CLIENT

insert into client
values(1, 'Smith', 'Maximilian', 'SmithMax@yahoo.com');

insert into client
values(2, 'Holt', 'Scarlet', 'HoltSc@yahoo.com');

insert into client
values(3, 'Munoz', 'Lottie', 'MunozLott@yahoo.com');

insert into client
values(4, 'Jane', 'Alexander', 'JaneAlex@yahoo.com');

insert into client
values(5, 'Phelps', 'Jason', 'JasonPhel@yahoo.com');

--INSERARE IN COMANDA

create sequence idcomanda_seq  
start with 1
increment by 1;

insert into comanda
values(idcomanda_seq.nextval, '1', to_date('2022-10-14', 'yyyy-mm-dd'), null);

insert into comanda
values(idcomanda_seq.nextval, '1', to_date('2022-10-15', 'yyyy-mm-dd'), 'Gingerbread coffee-ul sa nu aiba napolitane peste.');

insert into comanda
values(idcomanda_seq.nextval, '2', to_date('2022-10-14', 'yyyy-mm-dd'), null);

insert into comanda
values(idcomanda_seq.nextval, '2', to_date('2022-10-15','yyyy-mm-dd'), null);

insert into comanda
values(idcomanda_seq.nextval, '3', to_date('2022-10-14','yyyy-mm-dd'), null);

insert into comanda
values(idcomanda_seq.nextval, '3', to_date('2022-10-15','yyyy-mm-dd'), null);

insert into comanda
values(idcomanda_seq.nextval, '4', to_date('2022-10-15','yyyy-mm-dd'), null);

insert into comanda
values(idcomanda_seq.nextval, '5', to_date('2022-10-15','yyyy-mm-dd'), '4 cuburi de gheata in Iced Caramel Machiatto');

insert into comanda
values(idcomanda_seq.nextval, '5', to_date('2022-10-14','yyyy-mm-dd'), null);

--INSERARE IN ITEM_COMANDA

insert into item_comanda
values(1, 1, 1);

insert into item_comanda
values(1, 3, 1);

insert into item_comanda
values(2, 6, 1);

insert into item_comanda
values(3, 4, 1);

insert into item_comanda
values(4, 5, 2);

insert into item_comanda
values(5, 2, 1);

insert into item_comanda
values(5, 7, 2);

insert into item_comanda
values(6, 4, 2);

insert into item_comanda
values(7, 1, 4);

insert into item_comanda
values(8, 7, 1);

insert into item_comanda
values(9, 5, 2);

--INSERARE IN REVIEW

create sequence reviewid_seq
start with 1
increment by 1;

insert into review
values(reviewid_seq.nextval, 1, 'Servire 10/10', 5);

insert into review
values(reviewid_seq.nextval, 3, null, 4);

insert into review
values(reviewid_seq.nextval, 2, null, 3);

insert into review
values(reviewid_seq.nextval, 4, 'Local dragut, voi reveni! :D', 5);

insert into review
values(reviewid_seq.nextval, 5, 'The coffee was great', 5);

--inserare in gen

insert into gen
values(1, 'Actiune');

insert into gen
values(2, 'Sci-fi');

insert into gen
values(3, 'Aventura');

insert into gen
values(4, 'Non-fictiune');

insert into gen
values(5, 'Dragoste');

--inserari in carte

create sequence carteid_seq
start with 1
increment by 1;

insert into carte
values(carteid_seq.nextval, 1, 'Treasure Island', 'R. L. Stevenson',null);

insert into carte
values(carteid_seq.nextval, 1, 'Desert Star', 'Michael Connelly',null);

insert into carte
values(carteid_seq.nextval, 2, 'Dune', 'Frank Herbert',null);

insert into carte
values(carteid_seq.nextval, 2, 'Frankenstein', 'Mary Shelley',null);

insert into carte
values(carteid_seq.nextval, 2, 'Nineteen Eighty-Four', 'George Orwell',null);

insert into carte
values(carteid_seq.nextval, 3, 'The Call of the Wild', 'George Orwell',null);

insert into carte
values(carteid_seq.nextval, 3, 'Odyssey', 'Homer', null);

insert into carte
values(carteid_seq.nextval, 4, 'The Right Stuff', 'Tom Wolfe', null);

insert into carte
values(carteid_seq.nextval, 5, 'It Starts with Us', 'Colleen Hoover', null);

--INSERARE IN INVENTAR_CARTI

insert into inventar_carti
values(1, 1);

insert into inventar_carti
values(2, 2);

insert into inventar_carti
values(3, 1);

insert into inventar_carti
values(4, 2);

insert into inventar_carti
values(5, 3);

insert into inventar_carti
values(6, 1);

insert into inventar_carti
values(7, 3);

insert into inventar_carti
values(8, 1);

insert into inventar_carti
values(9, 2);

select * from inventar_carti;

--INSERARE IN IMPRUMUT

insert into imprumut
values (1,3, TO_DATE('2022-10-14 08:30:25', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-14 10:30:09', 'YYYY-MM-DD HH24:MI:SS'));
            
insert into imprumut
values (1,1, TO_DATE('2022-10-15 12:05:07', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-15 12:59:04', 'YYYY-MM-DD HH24:MI:SS'));
            
insert into imprumut
values (2,4, TO_DATE('2022-10-14 10:34:17', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-14 11:27:23', 'YYYY-MM-DD HH24:MI:SS'));
            
insert into imprumut
values (2,9, TO_DATE('2022-10-15 8:02:23', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-15 8:41:29', 'YYYY-MM-DD HH24:MI:SS'));

insert into imprumut
values (3,2, TO_DATE('2022-10-14 18:01:01', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-14 18:28:35', 'YYYY-MM-DD HH24:MI:SS'));
            
insert into imprumut
values (3,7, TO_DATE('2022-10-14 18:28:35', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-14 19:00:00', 'YYYY-MM-DD HH24:MI:SS'));
            
insert into imprumut
values (3,2, TO_DATE('2022-10-15 16:11:10', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-15 17:41:22', 'YYYY-MM-DD HH24:MI:SS'));

insert into imprumut
values (4,3, TO_DATE('2022-10-15 12:11:10', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-15 12:41:22', 'YYYY-MM-DD HH24:MI:SS'));

insert into imprumut
values (5,1, TO_DATE('2022-10-14 15:13:10', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-14 15:49:28', 'YYYY-MM-DD HH24:MI:SS'));

insert into imprumut
values (5,1, TO_DATE('2022-10-15 18:11:10', 'YYYY-MM-DD HH24:MI:SS'),
            TO_DATE('2022-10-15 19:00:00', 'YYYY-MM-DD HH24:MI:SS'));

--INSERT IN TIP_PROMOTIE

alter table cafea
modify nume_cafea varchar2(40);

alter table cafea
modify descriere varchar2(80);

alter table tip_promotie
modify detalii_promotie varchar2(80);

alter table tip_promotie
add tip_promotie_id number(3) not null;

alter table tip_promotie
drop column id_promotie;

alter table tip_promotie
add tip_promotie_id number(3) not null;

alter table tip_promotie
add primary key (tip_promotie_id);

alter table promotie
drop column id_promotie;

alter table promotie
add tip_promotie_id number(3) not null;

alter table promotie
add foreign key (tip_promotie_id) references tip_promotie(tip_promotie_id);

insert into tip_promotie
values('La 5 comenzi, primesti o cafea small gratuita.', 1); 

insert into tip_promotie
values('Daca lasi un review de 5 stele primesti 10% discount la urmatoarea ta comanda.', 2);

insert into promotie
values(1,1,2);

insert into promotie
values(7,4,2);

insert into promotie
values(9,5,2);

commit;

--6.
-- Sa se implementeze o procedura care afiseaza clientul care a citit cel mai mult timp
-- si numarul de ore pe care l-a citit.
-- Idee: inregistrez fiecare imprumut intr-un tabel imbricat cu 
-- in care retin durata timpului de citit si id-ul clientului in cauza.
-- Adun timpurile de citit retinute in tabelul imbricat intr-un tablou indexat.
-- Intr-un if clasic determin clientul care a citit cel mai mult.
create or replace procedure reading_time
is
    type c_r is record
    (
    client_id client.id_client%type,
    readingTime float --in ore
    );
    type hours_read is table of c_r;  --tablou imbricat
    client_read_time hours_read;
    type tr is table of c_r index by pls_integer; --tablou indexat 
    totalReading tr;
    maxReadingTime FLOAT := 0;
    idClientMax number(8);
    numeClient client.nume%type;
    prenumeClient client.prenume%type;
    idClient client.id_client%type;
begin
    select id_client, (data_restituire - data_imprumut)*24
    bulk collect into client_read_time
    from imprumut;
    
    for i in client_read_time.first..client_read_time.last loop
        idClient:= client_read_time(i).client_id;
        totalReading(idClient).client_id := client_read_time(i).client_id;
        totalReading(idClient).readingTime := 0;
    end loop;

    for i in client_read_time.first..client_read_time.last loop
        idClient:= client_read_time(i).client_id;
        totalReading(idClient).readingTime := totalReading(idClient).readingTime + client_read_time(i).readingTime;
        
        if totalReading(idClient).readingTime > maxReadingTime then
            maxReadingTime := totalReading(idClient).readingTime;
            idClientMax := idClient;
        end if;
    end loop;
    
    select c.nume, c.prenume into numeClient, prenumeClient
    from client c
    where idClientMax = c.id_client;
    
    dbms_output.put_line('Clientul ' || numeClient || ' ' || prenumeClient ||' a citit cel mai mult timp' ||
    ', in total ' || trunc(maxReadingTime,2) || ' ore.');
end;
/
begin
    reading_time();
end;
/
7.

--7.
--Pentru un client al carui nume e dat de la tastatura,
--sa se afiseze, totalul de ingrediente care a fost folosit
--in prepararea comenzilor lui, de-a lungul istoricului sau
--de comenzi
create or replace procedure coffee_times
(client_nume client.nume%type,
client_prenume client.prenume%type)
is 
    cursor c is 
    select id_comanda from comanda
    where id_client = (select id_client from client 
    where upper(nume) = upper(client_nume) 
    and upper(prenume) = upper(client_prenume));
    
    cursor c1(id_com comanda.id_comanda%type) is 
    select id_cafea, cantitate from item_comanda
    where id_comanda = id_com;
    
    cursor c2(idcaf cafea.id_cafea%type)
    is select id_ingredient, cantitate
    from contine_ingrediente where id_cafea = idcaf;
    
    cafeaCantitate c1%rowtype;
    ingredientCantitate c2%rowtype;
    
    type ingred is record
    (
    id_ing ingredient.id_ingredient%type,
    cantitate contine_ingrediente.cantitate%type
    );
    type ingTab is table of ingred;
    ingredientsTable ingTab := ingTab();
    nringred number(4);
    ingredientName ingredient.nume_ingredient%type;
    nrclienti number(4);
    notfoundexc exception;
begin
    select count(id_client) into nrclienti from client 
    where upper(nume) = upper(client_nume) 
    and upper(prenume) = upper(client_prenume);
    
    if nrclienti = 0 then
        raise notfoundexc; 
    end if;

    select count(*) into nringred 
    from ingredient;
    ingredientsTable.extend(nringred+1);
    for i in 1..nringred loop
        ingredientsTable(i).id_ing := i;
        ingredientsTable(i).cantitate := 0;
    end loop;
    for comanda_entry in c loop --ciclu cursor
        open c1(comanda_entry.id_comanda);
        loop
            fetch c1 into cafeaCantitate; --cursor clasic
            exit when c1%NOTFOUND;
                for ingredientCantitate in c2(cafeaCantitate.id_cafea) loop
                    --acum am: in comanda_entry Id comanda
                    --in cafeaCantitate id_cafea si cantitatea cafelei
                    --in ingredientCantitate id_ingredient si cantitatea ingredientului
                    ingredientsTable(ingredientCantitate.id_ingredient).cantitate := 
                    ingredientsTable(ingredientCantitate.id_ingredient).cantitate + 
                    cafeaCantitate.cantitate * ingredientCantitate.cantitate;
                end loop;
        end loop;
        close c1;
    end loop;
    dbms_output.put_line('Clientul cu numele ' || client_nume || ' ' || client_prenume || ' a comandat:');
    for i in ingredientsTable.first..ingredientsTable.last loop
        select nume_ingredient into ingredientName 
        from ingredient
        where id_ingredient = i;
        dbms_output.put_line(ingredientsTable(i).cantitate || ' ' || ingredientName);
    end loop;
    exception
    when notfoundexc then
    dbms_output.put_line('Clientul nu exista.');
    when others then
    dbms_output.put_line('');
end;
/
BEGIN
  coffee_times('Phelps','Jason');
END;
/

--8.
--Sa se scrie o functie care returneaza numarul mediu
--de comenzi facute de un client care a lasat un review
--de X stele (x fiind parametru dat), si afiseaza descriptiile review-urilor in cauza (daca 
--nu exista cel putin o descriere, se va apela o exceptie).

create or replace function avg_orders(stele review.nr_stele%type)
return float is 
no_reviews EXCEPTION;
PRAGMA exception_init(no_reviews, -20001);

no_descriptions EXCEPTION;
PRAGMA exception_init(no_descriptions, -20002);

orderNumber float;
type descriptionAndCount is record
(
id_client client.id_client%type,
countOfOrders number(6),
review_description review.descriptie%type
);
type descCount is table of descriptionAndCount index by pls_integer;
reviewTable descCount; 
numberOfOrders number(4) := 0;
numberOfClients number(4) :=0;
numberOfDescriptions number(4) :=0;
begin
    select count(*) into numberOfClients
    from review
    where nr_stele = stele;
    
    if numberOfClients =0 then
        raise_application_error(-20001, '');
    end if;
    numberOfClients := 0;
    select max(c.id_client), count(com.id_comanda), max(r.descriptie) bulk collect into reviewTable
    from review r 
    join client c 
    on r.id_client = c.id_client
    join comanda com 
    on com.id_client = c.id_client
    where r.nr_stele = stele
    group by c.id_client;
    dbms_output.put_line('Descrieri review-uri:');
    for i in reviewTable.first..reviewTable.last loop
       numberOfOrders := numberOfOrders + reviewTable(i).countOfOrders;
       numberOfClients := numberOfClients +1;
       if reviewTable(i).review_description is not null then
       dbms_output.put_line(reviewTable(i).review_description);
       numberOfDescriptions := numberOfDescriptions + 1;
       end if;
    end loop;
    if numberOfDescriptions =0 then
    raise_application_error(-20002,'');
    end if;
    orderNumber := numberOfOrders * 1.0 / numberOfClients;
    return trunc(orderNumber,3);
    exception
    when no_reviews then
    dbms_output.put_line('Niciun review cu numarul de stele specificat');
    return -1;
    when no_descriptions then
    dbms_output.put_line('Niciun review cu numarul de stele specificat care sa aiba descriere');
    return -1;
end avg_orders;
/
declare
orderNumberRet float;
begin
    orderNumberRet := avg_orders(2);
    dbms_output.put_line(orderNumberRet);
end;
/

--9. Pentru un client cu numele dat de la tastatura,
-- sa se verifice daca clientul a comandat o cafea venti si
-- a imprumutat o carte din categoria actiune in cadrul aceleasi vizite.
create or replace procedure client_check
(client_name client.nume%type)
is
    cursor c1(cid client.id_client%type) is 
    select c.id_client, caf.tip_cafea, g.nume_gen, com.data_comanda, imp.data_imprumut
    from client c 
    join comanda com 
    on com.id_client = c.id_client
    join item_comanda ic
    on ic.id_comanda = com.id_comanda
    join cafea caf
    on caf.id_cafea = ic.id_cafea
    join imprumut imp 
    on imp.id_client = c.id_client
    join carte car 
    on car.id_carte = imp.id_carte
    join gen g
    on g.id_gen = car.id_gen
    where id_client = cid;
    clientId client.id_client%type; 
    ok boolean := false;
begin
    select id_client into clientId
    from client c
    where upper(c.nume) = upper(client_name);
    
    for fullElem in c1(clientId) loop
        if fullElem.tip_cafea = 'Venti' and fullElem.nume_gen = 'Actiune' and 
        trunc(fullElem.data_comanda) = trunc(fullElem.data_imprumut) then
        ok := true;
        end if;
    end loop;
    
    if ok = true then
        dbms_output.put_line('Clientul ' || client_name || ' indeplineste criteriile.');
    else
        dbms_output.put_line('Clientul ' || client_name || ' nu indeplineste criteriile.');
    end if;
    exception
        when no_data_found then
        dbms_output.put_line('Niciun client cu numele specificat.');
        when too_many_rows then
        dbms_output.put_line('Mai multi clienti au numele specificat.');
end;
/

select * from client;
begin
    client_check('Phelps');
end;
/

10. 
--Definiti un trigger care sa introduca intr-un tabel nou definit de
-- notificari cate un anunt pentru fiecare ingredient al
-- carui stoc a scazut peste limita admisa.
create table notificari 
(
id_notificare number(3),
descriere varchar2(100),
data_notificare date,
constraint id_notificare_pk primary key (id_notificare)
);

create sequence notificari_seq
start with 1
increment by 1;

create or replace trigger forbidden_access
    after insert on item_comanda
declare
qantitate inventar_ingrediente.cantitate_disponibila%type;
numeIng ingredient.nume_ingredient%type;
begin
    for i in 1..9 loop 
        select cantitate_disponibila into qantitate
        from inventar_ingrediente
        where id_ingredient = i;
        
        if i=1 and qantitate <5000 then
            insert into notificari values (notificari_seq.nextval, 'Trebuie cumparata cafea.', sysdate);
            
        elsif i=2 and qantitate <2500 then
            insert into notificari values (notificari_seq.nextval, 'Trebuie cumparat lapte.', sysdate);
            
        elsif i>2 and i<8 and qantitate <500 then
            select nume_ingredient into numeIng
            from ingredient
            where id_ingredient = i;
            
            insert into notificari values(notificari_seq.nextval, 'Trebuie cumparat' || numeIng, sysdate);
        elsif i=8 and qantitate <100 then
            insert into notificari values(notificari_seq.nextval, 'Trebuie cumparate napolitane.', sysdate);
        elsif i=9 and qantitate <5 then
            insert into notificari values(notificari_seq.nextval, 'Trebuie cumparate cuburi de gheata.', SYSDATE);
        end if;
    end loop;
end;
/
begin
    insert into item_comanda values (10, 7, 33);
end;
/
select * from notificari;
commit;

--11. Creati un trigger care, odata introdusa o cafea ca membra a unei comenzi,
-- scade din inventarul ingredientelor pe care le contine cantitatile 
-- respective.

create or replace trigger triggerInventory
    after insert on item_comanda
    for each row
declare
    cursor c(id_caf cafea.id_cafea%type) is select 
    id_ingredient, cantitate from contine_ingrediente ci
    where ci.id_cafea = id_caf; 
    c_d inventar_ingrediente.cantitate_disponibila%type;
begin
    for i in c(:new.id_cafea) loop
        select cantitate_disponibila into c_d
        from inventar_ingrediente
        where id_ingredient = i.id_ingredient;
        if c_d < :new.cantitate * i.cantitate then
            rollback;
            RAISE_APPLICATION_ERROR (-20010, 'Nu sunt destule ingrediente pentru prepararea comenzii');
        else
            update inventar_ingrediente
            set cantitate_disponibila = cantitate_disponibila - :new.cantitate * i.cantitate
            where id_ingredient = i.id_ingredient;
        end if;
    end loop;
end;

select * from inventar_ingrediente;

insert into comanda values(10, 1, to_date('16-10-2022', 'DD-MM-YYYY'), null);
insert into item_comanda values(10, 5, 2);
/

commit;

--12.

--Definiti un trigger de tip LDD care sa retina intr-un tabel auxiliar istoricul
--comenzilor de tip LDD apelate de catre un user.
create table user_history
(
user_name varchar2(20),
event      varchar2(20),  
obj_name varchar2(20),
data      date     
);

create or replace trigger ldd_trigger
after create or alter or drop on schema
begin
    insert into user_history values(SYS.LOGIN_USER, SYS.SYSEVENT, SYS.DICTIONARY_OBJ_NAME, SYSDATE);
end;

create table something(
nimic varchar2(10)
);
/
select * from user_history;

--13.
create or replace package pachet_sgbd as
--6.
procedure reading_time;
--7.
procedure coffee_times (client_nume client.nume%type, 
                        client_prenume client.prenume%type);
--8.
function avg_orders(stele review.nr_stele%type) 
    return float;
--9.
procedure client_check (client_name client.nume%type);
end pachet_sgbd;
/
create or replace package body pachet_sgbd as
--6.
procedure reading_time
is
    type c_r is record
    (
    client_id client.id_client%type,
    readingTime float --in ore
    );
    type hours_read is table of c_r;  --tablou imbricat
    client_read_time hours_read;
    type tr is table of c_r index by pls_integer; --tablou indexat 
    totalReading tr;
    maxReadingTime FLOAT := 0;
    idClientMax number(8);
    numeClient client.nume%type;
    prenumeClient client.prenume%type;
    idClient client.id_client%type;
begin
    select id_client, (data_restituire - data_imprumut)*24
    bulk collect into client_read_time
    from imprumut;
    
    for i in client_read_time.first..client_read_time.last loop
        idClient:= client_read_time(i).client_id;
        totalReading(idClient).client_id := client_read_time(i).client_id;
        totalReading(idClient).readingTime := 0;
    end loop;

    for i in client_read_time.first..client_read_time.last loop
        idClient:= client_read_time(i).client_id;
        totalReading(idClient).readingTime := totalReading(idClient).readingTime + client_read_time(i).readingTime;
        
        if totalReading(idClient).readingTime > maxReadingTime then
            maxReadingTime := totalReading(idClient).readingTime;
            idClientMax := idClient;
        end if;
    end loop;
    
    select c.nume, c.prenume into numeClient, prenumeClient
    from client c
    where idClientMax = c.id_client;
    
    dbms_output.put_line('Clientul ' || numeClient || ' ' || prenumeClient ||' a citit cel mai mult timp' ||
    ', in total ' || trunc(maxReadingTime,2) || ' ore.');
end;
--7.
procedure coffee_times
(client_nume client.nume%type,
client_prenume client.prenume%type)
is 
    cursor c is 
    select id_comanda from comanda
    where id_client = (select id_client from client 
    where upper(nume) = upper(client_nume) 
    and upper(prenume) = upper(client_prenume));
    
    cursor c1(id_com comanda.id_comanda%type) is 
    select id_cafea, cantitate from item_comanda
    where id_comanda = id_com;
    
    cursor c2(idcaf cafea.id_cafea%type)
    is select id_ingredient, cantitate
    from contine_ingrediente where id_cafea = idcaf;
    
    cafeaCantitate c1%rowtype;
    ingredientCantitate c2%rowtype;
    
    type ingred is record
    (
    id_ing ingredient.id_ingredient%type,
    cantitate contine_ingrediente.cantitate%type
    );
    type ingTab is table of ingred;
    ingredientsTable ingTab := ingTab();
    nringred number(4);
    ingredientName ingredient.nume_ingredient%type;
    nrclienti number(4);
    notfoundexc exception;
begin
    select count(id_client) into nrclienti from client 
    where upper(nume) = upper(client_nume) 
    and upper(prenume) = upper(client_prenume);
    
    if nrclienti = 0 then
        raise notfoundexc; 
    end if;

    select count(*) into nringred 
    from ingredient;
    ingredientsTable.extend(nringred+1);
    for i in 1..nringred loop
        ingredientsTable(i).id_ing := i;
        ingredientsTable(i).cantitate := 0;
    end loop;
    for comanda_entry in c loop --ciclu cursor
        open c1(comanda_entry.id_comanda);
        loop
            fetch c1 into cafeaCantitate; --cursor clasic
            exit when c1%NOTFOUND;
                for ingredientCantitate in c2(cafeaCantitate.id_cafea) loop
                    --acum am: in comanda_entry Id comanda
                    --in cafeaCantitate id_cafea si cantitatea cafelei
                    --in ingredientCantitate id_ingredient si cantitatea ingredientului
                    ingredientsTable(ingredientCantitate.id_ingredient).cantitate := 
                    ingredientsTable(ingredientCantitate.id_ingredient).cantitate + 
                    cafeaCantitate.cantitate * ingredientCantitate.cantitate;
                end loop;
        end loop;
        close c1;
    end loop;
    dbms_output.put_line('Clientul cu numele ' || client_nume || ' ' || client_prenume || ' a comandat:');
    for i in ingredientsTable.first..ingredientsTable.last loop
        select nume_ingredient into ingredientName 
        from ingredient
        where id_ingredient = i;
        dbms_output.put_line(ingredientsTable(i).cantitate || ' ' || ingredientName);
    end loop;
    exception
    when notfoundexc then
    dbms_output.put_line('Clientul nu exista.');
    when others then
    dbms_output.put_line('');
end;

--8.
function avg_orders(stele review.nr_stele%type)
return float is 
no_reviews EXCEPTION;
PRAGMA exception_init(no_reviews, -20001);

no_descriptions EXCEPTION;
PRAGMA exception_init(no_descriptions, -20002);

orderNumber float;
type descriptionAndCount is record
(
id_client client.id_client%type,
countOfOrders number(6),
review_description review.descriptie%type
);
type descCount is table of descriptionAndCount index by pls_integer;
reviewTable descCount; 
numberOfOrders number(4) := 0;
numberOfClients number(4) :=0;
numberOfDescriptions number(4) :=0;
begin
    select count(*) into numberOfClients
    from review
    where nr_stele = stele;
    
    if numberOfClients =0 then
        raise_application_error(-20001, '');
    end if;
    numberOfClients := 0;
    select max(c.id_client), count(com.id_comanda), max(r.descriptie) bulk collect into reviewTable
    from review r 
    join client c 
    on r.id_client = c.id_client
    join comanda com 
    on com.id_client = c.id_client
    where r.nr_stele = stele
    group by c.id_client;
    dbms_output.put_line('Descrieri review-uri:');
    for i in reviewTable.first..reviewTable.last loop
       numberOfOrders := numberOfOrders + reviewTable(i).countOfOrders;
       numberOfClients := numberOfClients +1;
       if reviewTable(i).review_description is not null then
       dbms_output.put_line(reviewTable(i).review_description);
       numberOfDescriptions := numberOfDescriptions + 1;
       end if;
    end loop;
    if numberOfDescriptions =0 then
    raise_application_error(-20002,'');
    end if;
    orderNumber := numberOfOrders * 1.0 / numberOfClients;
    return trunc(orderNumber,3);
    exception
    when no_reviews then
    dbms_output.put_line('Niciun review cu numarul de stele specificat');
    return -1;
    when no_descriptions then
    dbms_output.put_line('Niciun review cu numarul de stele specificat care sa aiba descriere');
    return -1;
end avg_orders;

--9.
procedure client_check
(client_name client.nume%type)
is
    cursor c1(cid client.id_client%type) is 
    select c.id_client, caf.tip_cafea, g.nume_gen, com.data_comanda, imp.data_imprumut
    from client c 
    join comanda com 
    on com.id_client = c.id_client
    join item_comanda ic
    on ic.id_comanda = com.id_comanda
    join cafea caf
    on caf.id_cafea = ic.id_cafea
    join imprumut imp 
    on imp.id_client = c.id_client
    join carte car 
    on car.id_carte = imp.id_carte
    join gen g
    on g.id_gen = car.id_gen
    where id_client = cid;
    clientId client.id_client%type; 
    ok boolean := false;
begin
    select id_client into clientId
    from client c
    where upper(c.nume) = upper(client_name);
    
    for fullElem in c1(clientId) loop
        if fullElem.tip_cafea = 'Venti' and fullElem.nume_gen = 'Actiune' and 
        trunc(fullElem.data_comanda) = trunc(fullElem.data_imprumut) then
        ok := true;
        end if;
    end loop;
    
    if ok = true then
        dbms_output.put_line('Clientul ' || client_name || ' indeplineste criteriile.');
    else
        dbms_output.put_line('Clientul ' || client_name || ' nu indeplineste criteriile.');
    end if;
    exception
        when no_data_found then
        dbms_output.put_line('Niciun client cu numele specificat.');
        when too_many_rows then
        dbms_output.put_line('Mai multi clienti au numele specificat.');
end;
end pachet_sgbd;

/
begin
    pachet_sgbd.reading_time();
end;
/
BEGIN
  pachet_sgbd.coffee_times('Phelps','Jason');
END;
/
declare
orderNumberRet float;
begin
    orderNumberRet := avg_orders(2);
    dbms_output.put_line(orderNumberRet);
end;
/
begin
    pachet_sgbd.client_check('Phelps');
end;
/



