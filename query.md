### Query
#### SELECT

    1. Selezionare tutte le software house americane (3)


        ```sql
        select *
        from software_houses sh 
        where sh.country like 'United States';
        ```

    2. Selezionare tutti i giocatori della cittÃ  di 'Rogahnland' (2)
        select * 
        from players p 
        where p.city like 'Rogahnland';

    3. Selezionare tutti i giocatori il cui nome finisce per "a" (220)
        select * 
        from players p 
        where p.name like '%a';

    4. Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)
        select * 
        from reviews r 
        where r.player_id = 800;

    5. Contare quanti tornei ci sono stati nell'anno 2015 (9)
        select * 
        from tournaments t 
        where t.year = 2015;

    6. Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)
        select * 
        from awards a 
        where a.description like '%facere%';

    7. Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)
        select distinct videogame_id  
        from category_videogame cv 
        where cv.category_id = 2 
        or cv.category_id = 6;

    8. Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)
        select *
        from reviews r 
        where r.rating between 2 and 4;

    9. Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)
        select *
        from videogames v 
        where year(v.release_date) = 2020;

    10. Selezionare gli id dei videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)
        select distinct r.videogame_id 
        from reviews r 
        where r.rating = 5;

##### BONUS
    11. Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)
        select count(id), avg(r.rating)
        from reviews r 
        where r.videogame_id = 412;

    12. Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)
        select *
        from videogames v 
        where v.software_house_id = 1
        and year(v.release_date) = 2018;

#### GROUP BY
    1. Contare quante software house ci sono per ogni paese (3)
        select sh.country
        from software_houses sh 
        group by sh.country; 

    2. Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)
        select r.videogame_id, count(r.rating)
        from reviews r  
        group by r.videogame_id; 

    3. Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)
        select plv.pegi_label_id, count(plv.videogame_id)
        from pegi_label_videogame plv 
        group by plv.pegi_label_id;

    4. Mostrare il numero di videogiochi rilasciati ogni anno (11)
        select year(v.release_date)
        from videogames v 
        group by year(v.release_date);

    5. Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)
        select dv.device_id , count(dv.videogame_id) 
        from device_videogame dv 
        group by dv.device_id;

    6. Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)
        select r.videogame_id , avg(r.rating)  
        from reviews r 
        group by r.videogame_id order by avg(r.rating) desc; 



#### JOIN
    1. Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)
        select distinct p.*
        from reviews r  
        join players p  on r.player_id = p.id;

    2. Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)
        select distinct v.id, v.name
        from tournaments t 
        join tournament_videogame tv on t.id = tv.tournament_id
        join videogames v on tv.videogame_id = v.id 
        where t.year = 2016;

    3. Mostrare le categorie di ogni videogioco (1718)
        select v.name, cv.category_id 
        from videogames v 
        join category_videogame cv on v.id = cv.videogame_id  

    4. Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)
        select distinct sh.name
        from software_houses sh 
        join videogames v on sh.id = v.software_house_id 
        where year(v.release_date) > 2020

    5. Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)
        select *
        from software_houses sh 
        join videogames v on sh.id = v.software_house_id 
        join award_videogame av on v.id = av.videogame_id 

    6. Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)
        select distinct v.name, c.name, pl.name
        from videogames v 
        join reviews r on v.id = r.videogame_id 
        join category_videogame cv on v.id = cv.videogame_id 
        join categories c on cv.category_id = c.id
        join pegi_label_videogame plv on v.id = plv.videogame_id 
        join pegi_labels pl on plv.pegi_label_id = pl.id 
        where r.rating = 4
        or r.rating = 5;

    7. Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)
        select distinct v.name
        from videogames v 
        join tournament_videogame tv on v.id = tv.videogame_id 
        join tournaments t on tv.tournament_id = t.id 
        join player_tournament pt on t.id = pt.tournament_id 
        join players p on pt.player_id = p.id 
        where p.name like 's%';

        mi esce 473 🙃

    8. Selezionare le citta  in cui  stato giocato il gioco dell'anno del 2018 (36)
    9. Selezionare i giocatori che hanno giocato al gioco piÃ¹ atteso del 2018 in un torneo del 2019 (3306)

##### BONUS
    10. Selezionare i dati della prima software house che ha rilasciato un gioco, assieme ai dati del gioco stesso (software house id : 5)
    11. Selezionare i dati del videogame (id, name, release_date, totale recensioni) con piÃ¹ recensioni (videogame id : potrebbe uscire 449 o 398, sono entrambi a 20)
    12. Selezionare la software house che ha vinto piÃ¹ premi tra il 2015 e il 2016 (software house id : potrebbe uscire 3 o 1, sono entrambi a 3)
    13. Selezionare le categorie dei videogame i quali hanno una media recensioni inferiore a 2 (10)