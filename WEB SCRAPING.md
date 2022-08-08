Write a python program to display all the header tags from wikipedia.org.


```python
import  requests
from  bs4 import BeautifulSoup

url = requests.get('https://en.wikipedia.org/wiki/Main_Page')
soup = BeautifulSoup(url.text, 'html.parser')
story = soup.find_all(['h1', 'h2','h3'])
for i in story:
    print(i)
```

    <h1 class="firstHeading mw-first-heading" id="firstHeading" style="display: none">Main Page</h1>
    <h1><span class="mw-headline" id="Welcome_to_Wikipedia">Welcome to <a href="/wiki/Wikipedia" title="Wikipedia">Wikipedia</a></span></h1>
    <h2 class="mp-h2" id="mp-tfa-h2"><span id="From_today.27s_featured_article"></span><span class="mw-headline" id="From_today's_featured_article">From today's featured article</span></h2>
    <h2 class="mp-h2" id="mp-dyk-h2"><span class="mw-headline" id="Did_you_know_...">Did you know ...</span></h2>
    <h2 class="mp-h2" id="mp-itn-h2"><span class="mw-headline" id="In_the_news">In the news</span></h2>
    <h2 class="mp-h2" id="mp-otd-h2"><span class="mw-headline" id="On_this_day">On this day</span></h2>
    <h2 class="mp-h2" id="mp-tfl-h2"><span id="From_today.27s_featured_list"></span><span class="mw-headline" id="From_today's_featured_list">From today's featured list</span></h2>
    <h2 class="mp-h2" id="mp-tfp-h2"><span id="Today.27s_featured_picture"></span><span class="mw-headline" id="Today's_featured_picture">Today's featured picture</span></h2>
    <h2 class="mp-h2" id="mp-other"><span class="mw-headline" id="Other_areas_of_Wikipedia">Other areas of Wikipedia</span></h2>
    <h2 class="mp-h2" id="mp-sister"><span id="Wikipedia.27s_sister_projects"></span><span class="mw-headline" id="Wikipedia's_sister_projects">Wikipedia's sister projects</span></h2>
    <h2 class="mp-h2" id="mp-lang"><span class="mw-headline" id="Wikipedia_languages">Wikipedia languages</span></h2>
    <h2>Navigation menu</h2>
    <h3 class="vector-menu-heading" id="p-personal-label">
    <span class="vector-menu-heading-label">Personal tools</span>
    </h3>
    <h3 class="vector-menu-heading" id="p-namespaces-label">
    <span class="vector-menu-heading-label">Namespaces</span>
    </h3>
    <h3 class="vector-menu-heading" id="p-views-label">
    <span class="vector-menu-heading-label">Views</span>
    </h3>
    <h3>
    <label for="searchInput">Search</label>
    </h3>
    <h3 class="vector-menu-heading" id="p-navigation-label">
    <span class="vector-menu-heading-label">Navigation</span>
    </h3>
    <h3 class="vector-menu-heading" id="p-interaction-label">
    <span class="vector-menu-heading-label">Contribute</span>
    </h3>
    <h3 class="vector-menu-heading" id="p-tb-label">
    <span class="vector-menu-heading-label">Tools</span>
    </h3>
    <h3 class="vector-menu-heading" id="p-coll-print_export-label">
    <span class="vector-menu-heading-label">Print/export</span>
    </h3>
    <h3 class="vector-menu-heading" id="p-wikibase-otherprojects-label">
    <span class="vector-menu-heading-label">In other projects</span>
    </h3>
    <h3 class="vector-menu-heading" id="p-lang-label">
    <span class="vector-menu-heading-label">Languages</span>
    </h3>
    

Write a python program to display IMDB’s Top rated 100 Indian movies’ data (i.e. name, rating, year of
release) and make data frame.


```python
from bs4 import BeautifulSoup
import requests
import re
import pandas as pd
 
 
# Downloading imdb top 250 movie's data
url = 'http://www.imdb.com/chart/top'
response = requests.get(url)
soup = BeautifulSoup(response.text, "html.parser")
movies = soup.select('td.titleColumn')
crew = [a.attrs.get('title') for a in soup.select('td.titleColumn a')]
ratings = [b.attrs.get('data-value')
        for b in soup.select('td.posterColumn span[name=ir]')]
 
 
 
 
# create a empty list for storing
# movie information
list = []
 
# Iterating over movies to extract
# each movie's details
for index in range(0, len(movies)):
     
    # Separating movie into: 'place',
    # 'title', 'year'
    movie_string = movies[index].get_text()
    movie = (' '.join(movie_string.split()).replace('.', ''))
    movie_title = movie[len(str(index))+1:-7]
    year = re.search('\((.*?)\)', movie_string).group(1)
    place = movie[:len(str(index))-(len(movie))]
    data = {"place": place,
            "movie_title": movie_title,
            "rating": ratings[index],
            "year": year,
            "star_cast": crew[index],
            }
    list.append(data)
 
# printing movie details with its rating.
for movie in list:
    print(movie['place'], '-', movie['movie_title'], '('+movie['year'] +
        ') -', 'Starring:', movie['star_cast'], movie['rating'])
 
 
##.......##
df = pd.DataFrame(list)
df.to_csv('imdb_top_250_movies.csv',index=False)
```

    1 - The Shawshank Redemption (1994) - Starring: Frank Darabont (dir.), Tim Robbins, Morgan Freeman 9.234567902165335
    2 - The Godfather (1972) - Starring: Francis Ford Coppola (dir.), Marlon Brando, Al Pacino 9.156458550018034
    3 - The Dark Knight (2008) - Starring: Christopher Nolan (dir.), Christian Bale, Heath Ledger 8.986915907670989
    4 - The Godfather Part II (1974) - Starring: Francis Ford Coppola (dir.), Al Pacino, Robert De Niro 8.984646145261827
    5 - 12 Angry Men (1957) - Starring: Sidney Lumet (dir.), Henry Fonda, Lee J. Cobb 8.949038527265344
    6 - Schindler's List (1993) - Starring: Steven Spielberg (dir.), Liam Neeson, Ralph Fiennes 8.936065922163364
    7 - The Lord of the Rings: The Return of the King (2003) - Starring: Peter Jackson (dir.), Elijah Wood, Viggo Mortensen 8.922773312964866
    8 - Pulp Fiction (1994) - Starring: Quentin Tarantino (dir.), John Travolta, Uma Thurman 8.850080759216153
    9 - The Lord of the Rings: The Fellowship of the Ring (2001) - Starring: Peter Jackson (dir.), Elijah Wood, Ian McKellen 8.80468823587367
    1 -  Il buono, il brutto, il cattivo (1966) - Starring: Sergio Leone (dir.), Clint Eastwood, Eli Wallach 8.793117869924743
    11 - Forrest Gump (1994) - Starring: Robert Zemeckis (dir.), Tom Hanks, Robin Wright 8.767601379661228
    12 - Fight Club (1999) - Starring: David Fincher (dir.), Brad Pitt, Edward Norton 8.749471360821582
    13 - Inception (2010) - Starring: Christopher Nolan (dir.), Leonardo DiCaprio, Joseph Gordon-Levitt 8.732965574363785
    14 - The Lord of the Rings: The Two Towers (2002) - Starring: Peter Jackson (dir.), Elijah Wood, Ian McKellen 8.72914440539225
    15 - The Empire Strikes Back (1980) - Starring: Irvin Kershner (dir.), Mark Hamill, Harrison Ford 8.701505943958994
    16 - The Matrix (1999) - Starring: Lana Wachowski (dir.), Keanu Reeves, Laurence Fishburne 8.670746683142783
    17 - Goodfellas (1990) - Starring: Martin Scorsese (dir.), Robert De Niro, Ray Liotta 8.652393327877666
    18 - One Flew Over the Cuckoo's Nest (1975) - Starring: Milos Forman (dir.), Jack Nicholson, Louise Fletcher 8.64064533844586
    19 - Se7en (1995) - Starring: David Fincher (dir.), Morgan Freeman, Brad Pitt 8.604314329912118
    20 - Shichinin no samurai (1954) - Starring: Akira Kurosawa (dir.), Toshirô Mifune, Takashi Shimura 8.599452024670258
    21 - It's a Wonderful Life (1946) - Starring: Frank Capra (dir.), James Stewart, Donna Reed 8.597096765502316
    22 - The Silence of the Lambs (1991) - Starring: Jonathan Demme (dir.), Jodie Foster, Anthony Hopkins 8.588301698085317
    23 - Cidade de Deus (2002) - Starring: Fernando Meirelles (dir.), Alexandre Rodrigues, Leandro Firmino 8.579003955465561
    24 - Saving Private Ryan (1998) - Starring: Steven Spielberg (dir.), Tom Hanks, Matt Damon 8.578357903366582
    25 - La vita è bella (1997) - Starring: Roberto Benigni (dir.), Roberto Benigni, Nicoletta Braschi 8.566374586542988
    26 - The Green Mile (1999) - Starring: Frank Darabont (dir.), Tom Hanks, Michael Clarke Duncan 8.56058714301237
    27 - Interstellar (2014) - Starring: Christopher Nolan (dir.), Matthew McConaughey, Anne Hathaway 8.558407308944934
    28 - Star Wars (1977) - Starring: George Lucas (dir.), Mark Hamill, Harrison Ford 8.553417038020184
    29 - Terminator 2: Judgment Day (1991) - Starring: James Cameron (dir.), Arnold Schwarzenegger, Linda Hamilton 8.536039947711672
    30 - Back to the Future (1985) - Starring: Robert Zemeckis (dir.), Michael J. Fox, Christopher Lloyd 8.518231810391068
    31 - Sen to Chihiro no kamikakushi (2001) - Starring: Hayao Miyazaki (dir.), Daveigh Chase, Suzanne Pleshette 8.51529059234495
    32 - Psycho (1960) - Starring: Alfred Hitchcock (dir.), Anthony Perkins, Janet Leigh 8.508509762278294
    33 - The Pianist (2002) - Starring: Roman Polanski (dir.), Adrien Brody, Thomas Kretschmann 8.505221064965651
    34 - Léon (1994) - Starring: Luc Besson (dir.), Jean Reno, Gary Oldman 8.499263665127222
    35 - Gisaengchung (2019) - Starring: Bong Joon Ho (dir.), Song Kang-ho, Sun-kyun Lee 8.495814241923254
    36 - The Lion King (1994) - Starring: Roger Allers (dir.), Matthew Broderick, Jeremy Irons 8.488557780458082
    37 - Gladiator (2000) - Starring: Ridley Scott (dir.), Russell Crowe, Joaquin Phoenix 8.486208529972036
    38 - American History X (1998) - Starring: Tony Kaye (dir.), Edward Norton, Edward Furlong 8.48405674046955
    39 - The Departed (2006) - Starring: Martin Scorsese (dir.), Leonardo DiCaprio, Matt Damon 8.475585012927635
    40 - The Usual Suspects (1995) - Starring: Bryan Singer (dir.), Kevin Spacey, Gabriel Byrne 8.475240558796429
    41 - The Prestige (2006) - Starring: Christopher Nolan (dir.), Christian Bale, Hugh Jackman 8.468415965781379
    42 - Casablanca (1942) - Starring: Michael Curtiz (dir.), Humphrey Bogart, Ingrid Bergman 8.464969019616372
    43 - Whiplash (2014) - Starring: Damien Chazelle (dir.), Miles Teller, J.K. Simmons 8.46173028705754
    44 - The Intouchables (2011) - Starring: Olivier Nakache (dir.), François Cluzet, Omar Sy 8.453649935544153
    45 - Seppuku (1962) - Starring: Masaki Kobayashi (dir.), Tatsuya Nakadai, Akira Ishihama 8.450560278909615
    46 - Hotaru no haka (1988) - Starring: Isao Takahata (dir.), Tsutomu Tatsumi, Ayano Shiraishi 8.449892870907497
    47 - Modern Times (1936) - Starring: Charles Chaplin (dir.), Charles Chaplin, Paulette Goddard 8.449696327411084
    48 - Once Upon a Time in the West (1968) - Starring: Sergio Leone (dir.), Henry Fonda, Charles Bronson 8.447314819595174
    49 - Top Gun: Maverick (2022) - Starring: Joseph Kosinski (dir.), Tom Cruise, Jennifer Connelly 8.44310283380061
    50 - Rear Window (1954) - Starring: Alfred Hitchcock (dir.), James Stewart, Grace Kelly 8.437337748773944
    51 - Alien (1979) - Starring: Ridley Scott (dir.), Sigourney Weaver, Tom Skerritt 8.43518165303384
    52 - City Lights (1931) - Starring: Charles Chaplin (dir.), Charles Chaplin, Virginia Cherrill 8.434761177418798
    53 - Nuovo Cinema Paradiso (1988) - Starring: Giuseppe Tornatore (dir.), Philippe Noiret, Enzo Cannavale 8.430794277877203
    54 - Apocalypse Now (1979) - Starring: Francis Ford Coppola (dir.), Martin Sheen, Marlon Brando 8.424969582767586
    55 - Memento (2000) - Starring: Christopher Nolan (dir.), Guy Pearce, Carrie-Anne Moss 8.424960582828607
    56 - Raiders of the Lost Ark (1981) - Starring: Steven Spielberg (dir.), Harrison Ford, Karen Allen 8.411278430460282
    57 - Django Unchained (2012) - Starring: Quentin Tarantino (dir.), Jamie Foxx, Christoph Waltz 8.403731271348732
    58 - WALL·E (2008) - Starring: Andrew Stanton (dir.), Ben Burtt, Elissa Knight 8.393149486089575
    59 - The Lives of Others (2006) - Starring: Florian Henckel von Donnersmarck (dir.), Ulrich Mühe, Martina Gedeck 8.386289610365015
    60 - Sunset Blvd (1950) - Starring: Billy Wilder (dir.), William Holden, Gloria Swanson 8.383098816372836
    61 - Paths of Glory (1957) - Starring: Stanley Kubrick (dir.), Kirk Douglas, Ralph Meeker 8.371641235551241
    62 - The Shining (1980) - Starring: Stanley Kubrick (dir.), Jack Nicholson, Shelley Duvall 8.367312332910176
    63 - The Great Dictator (1940) - Starring: Charles Chaplin (dir.), Charles Chaplin, Paulette Goddard 8.366368735560878
    64 - Witness for the Prosecution (1957) - Starring: Billy Wilder (dir.), Tyrone Power, Marlene Dietrich 8.359620481984578
    65 - Avengers: Infinity War (2018) - Starring: Anthony Russo (dir.), Robert Downey Jr., Chris Hemsworth 8.357563163764897
    66 - Aliens (1986) - Starring: James Cameron (dir.), Sigourney Weaver, Michael Biehn 8.344830529766618
    67 - American Beauty (1999) - Starring: Sam Mendes (dir.), Kevin Spacey, Annette Bening 8.33649481950344
    68 - Dr Strangelove or: How I Learned to Stop Worrying and Love the Bomb (1964) - Starring: Stanley Kubrick (dir.), Peter Sellers, George C. Scott 8.332754937483173
    69 - Spider-Man: Into the Spider-Verse (2018) - Starring: Bob Persichetti (dir.), Shameik Moore, Jake Johnson 8.330083597600845
    70 - The Dark Knight Rises (2012) - Starring: Christopher Nolan (dir.), Christian Bale, Tom Hardy 8.32830580778164
    71 - Oldeuboi (2003) - Starring: Park Chan-wook (dir.), Choi Min-sik, Yoo Ji-tae 8.319891663897895
    72 - Joker (2019) - Starring: Todd Phillips (dir.), Joaquin Phoenix, Robert De Niro 8.318275387688319
    73 - Amadeus (1984) - Starring: Milos Forman (dir.), F. Murray Abraham, Tom Hulce 8.31585891719635
    74 - Braveheart (1995) - Starring: Mel Gibson (dir.), Mel Gibson, Sophie Marceau 8.315538421558491
    75 - Toy Story (1995) - Starring: John Lasseter (dir.), Tom Hanks, Tim Allen 8.315109167072308
    76 - Coco (2017) - Starring: Lee Unkrich (dir.), Anthony Gonzalez, Gael García Bernal 8.312880099962873
    77 - Das Boot (1981) - Starring: Wolfgang Petersen (dir.), Jürgen Prochnow, Herbert Grönemeyer 8.312733001270862
    78 - Inglourious Basterds (2009) - Starring: Quentin Tarantino (dir.), Brad Pitt, Diane Kruger 8.312284588766133
    79 - Mononoke-hime (1997) - Starring: Hayao Miyazaki (dir.), Yôji Matsuda, Yuriko Ishida 8.303556318290518
    80 - Avengers: Endgame (2019) - Starring: Anthony Russo (dir.), Robert Downey Jr., Chris Evans 8.301594996833389
    81 - Once Upon a Time in America (1984) - Starring: Sergio Leone (dir.), Robert De Niro, James Woods 8.298945229681959
    82 - Good Will Hunting (1997) - Starring: Gus Van Sant (dir.), Robin Williams, Matt Damon 8.28760107950885
    83 - Kimi no na wa (2016) - Starring: Makoto Shinkai (dir.), Ryûnosuke Kamiki, Mone Kamishiraishi 8.276993447421301
    84 - Requiem for a Dream (2000) - Starring: Darren Aronofsky (dir.), Ellen Burstyn, Jared Leto 8.276112479794865
    85 - Toy Story 3 (2010) - Starring: Lee Unkrich (dir.), Tom Hanks, Tim Allen 8.276094453367868
    86 - Singin' in the Rain (1952) - Starring: Stanley Donen (dir.), Gene Kelly, Donald O'Connor 8.274116597595318
    87 - 3 Idiots (2009) - Starring: Rajkumar Hirani (dir.), Aamir Khan, Madhavan 8.27061191485976
    88 - Star Wars: Episode VI - Return of the Jedi (1983) - Starring: Richard Marquand (dir.), Mark Hamill, Harrison Ford 8.267540775538553
    89 - Tengoku to jigoku (1963) - Starring: Akira Kurosawa (dir.), Toshirô Mifune, Yutaka Sada 8.265221599638831
    90 - 2001: A Space Odyssey (1968) - Starring: Stanley Kubrick (dir.), Keir Dullea, Gary Lockwood 8.264400830817989
    91 - Eternal Sunshine of the Spotless Mind (2004) - Starring: Michel Gondry (dir.), Jim Carrey, Kate Winslet 8.263818779949442
    92 - Reservoir Dogs (1992) - Starring: Quentin Tarantino (dir.), Harvey Keitel, Tim Roth 8.263553042740263
    93 - Capharnaüm (2018) - Starring: Nadine Labaki (dir.), Zain Al Rafeea, Yordanos Shiferaw 8.25955245961342
    94 - Lawrence of Arabia (1962) - Starring: David Lean (dir.), Peter O'Toole, Alec Guinness 8.25689021074421
    95 - Citizen Kane (1941) - Starring: Orson Welles (dir.), Orson Welles, Joseph Cotten 8.25680507315158
    96 - Jagten (2012) - Starring: Thomas Vinterberg (dir.), Mads Mikkelsen, Thomas Bo Larsen 8.255569986051231
    97 - M - Eine Stadt sucht einen Mörder (1931) - Starring: Fritz Lang (dir.), Peter Lorre, Ellen Widmann 8.254635249243726
    98 - North by Northwest (1959) - Starring: Alfred Hitchcock (dir.), Cary Grant, Eva Marie Saint 8.25286836048396
    99 - Vertigo (1958) - Starring: Alfred Hitchcock (dir.), James Stewart, Kim Novak 8.247222673876806
    10 -  Idi i smotri (1985) - Starring: Elem Klimov (dir.), Aleksey Kravchenko, Olga Mironova 8.246870897054373
    101 - Le fabuleux destin d'Amélie Poulain (2001) - Starring: Jean-Pierre Jeunet (dir.), Audrey Tautou, Mathieu Kassovitz 8.244812370111761
    102 - A Clockwork Orange (1971) - Starring: Stanley Kubrick (dir.), Malcolm McDowell, Patrick Magee 8.243686563310467
    103 - Full Metal Jacket (1987) - Starring: Stanley Kubrick (dir.), Matthew Modine, R. Lee Ermey 8.239712856507326
    104 - Double Indemnity (1944) - Starring: Billy Wilder (dir.), Fred MacMurray, Barbara Stanwyck 8.23971025843979
    105 - The Apartment (1960) - Starring: Billy Wilder (dir.), Jack Lemmon, Shirley MacLaine 8.238782943443885
    106 - Scarface (1983) - Starring: Brian De Palma (dir.), Al Pacino, Michelle Pfeiffer 8.235549819703756
    107 - Ikiru (1952) - Starring: Akira Kurosawa (dir.), Takashi Shimura, Nobuo Kaneko 8.23513273653938
    108 - The Sting (1973) - Starring: George Roy Hill (dir.), Paul Newman, Robert Redford 8.22944593094846
    109 - To Kill a Mockingbird (1962) - Starring: Robert Mulligan (dir.), Gregory Peck, John Megna 8.228797964274591
    110 - Taxi Driver (1976) - Starring: Martin Scorsese (dir.), Robert De Niro, Jodie Foster 8.226464372540466
    111 - Up (2009) - Starring: Pete Docter (dir.), Edward Asner, Jordan Nagai 8.224618273897443
    112 - Heat (1995) - Starring: Michael Mann (dir.), Al Pacino, Robert De Niro 8.224118387852483
    113 - LA Confidential (1997) - Starring: Curtis Hanson (dir.), Kevin Spacey, Russell Crowe 8.223716506360725
    114 - Metropolis (1927) - Starring: Fritz Lang (dir.), Brigitte Helm, Alfred Abel 8.2219860224727
    115 - Jodaeiye Nader az Simin (2011) - Starring: Asghar Farhadi (dir.), Payman Maadi, Leila Hatami 8.220995276933614
    116 - Incendies (2010) - Starring: Denis Villeneuve (dir.), Lubna Azabal, Mélissa Désormeaux-Poulin 8.220876476510012
    117 - Die Hard (1988) - Starring: John McTiernan (dir.), Bruce Willis, Alan Rickman 8.220197768512152
    118 - Snatch (2000) - Starring: Guy Ritchie (dir.), Jason Statham, Brad Pitt 8.22008724546432
    119 - Hamilton (2020) - Starring: Thomas Kail (dir.), Lin-Manuel Miranda, Phillipa Soo 8.218935061980899
    120 - Ladri di biciclette (1948) - Starring: Vittorio De Sica (dir.), Lamberto Maggiorani, Enzo Staiola 8.218586502027827
    121 - Indiana Jones and the Last Crusade (1989) - Starring: Steven Spielberg (dir.), Harrison Ford, Sean Connery 8.218526337895414
    122 - 1917 (2019) - Starring: Sam Mendes (dir.), Dean-Charles Chapman, George MacKay 8.211793007942891
    123 - Taare Zameen Par (2007) - Starring: Aamir Khan (dir.), Darsheel Safary, Aamir Khan 8.207812144162608
    124 - Der Untergang (2004) - Starring: Oliver Hirschbiegel (dir.), Bruno Ganz, Alexandra Maria Lara 8.204942035867576
    125 - Per qualche dollaro in più (1965) - Starring: Sergio Leone (dir.), Clint Eastwood, Lee Van Cleef 8.203171560894285
    126 - Batman Begins (2005) - Starring: Christopher Nolan (dir.), Christian Bale, Michael Caine 8.201592798967111
    127 - Dangal (2016) - Starring: Nitesh Tiwari (dir.), Aamir Khan, Sakshi Tanwar 8.199465663135793
    128 - The Kid (1921) - Starring: Charles Chaplin (dir.), Charles Chaplin, Edna Purviance 8.19508387744374
    129 - Some Like It Hot (1959) - Starring: Billy Wilder (dir.), Marilyn Monroe, Tony Curtis 8.192471864718673
    130 - All About Eve (1950) - Starring: Joseph L. Mankiewicz (dir.), Bette Davis, Anne Baxter 8.181604212491283
    131 - Spider-Man: No Way Home (2021) - Starring: Jon Watts (dir.), Tom Holland, Zendaya 8.181509963328027
    132 - The Father (2020) - Starring: Florian Zeller (dir.), Anthony Hopkins, Olivia Colman 8.181138258041049
    133 - Green Book (2018) - Starring: Peter Farrelly (dir.), Viggo Mortensen, Mahershala Ali 8.174794732510026
    134 - The Wolf of Wall Street (2013) - Starring: Martin Scorsese (dir.), Leonardo DiCaprio, Jonah Hill 8.17293650908876
    135 - Judgment at Nuremberg (1961) - Starring: Stanley Kramer (dir.), Spencer Tracy, Burt Lancaster 8.170494319030283
    136 - Ran (1985) - Starring: Akira Kurosawa (dir.), Tatsuya Nakadai, Akira Terao 8.165980161397975
    137 - Casino (1995) - Starring: Martin Scorsese (dir.), Robert De Niro, Sharon Stone 8.164131540319735
    138 - Unforgiven (1992) - Starring: Clint Eastwood (dir.), Clint Eastwood, Gene Hackman 8.164080462496525
    139 - Pan's Labyrinth (2006) - Starring: Guillermo del Toro (dir.), Ivana Baquero, Ariadna Gil 8.162847409024536
    140 - There Will Be Blood (2007) - Starring: Paul Thomas Anderson (dir.), Daniel Day-Lewis, Paul Dano 8.156197001366738
    141 - The Sixth Sense (1999) - Starring: M. Night Shyamalan (dir.), Bruce Willis, Haley Joel Osment 8.15408570176011
    142 - The Truman Show (1998) - Starring: Peter Weir (dir.), Jim Carrey, Ed Harris 8.153457298716518
    143 - A Beautiful Mind (2001) - Starring: Ron Howard (dir.), Russell Crowe, Ed Harris 8.152830378348948
    144 - Monty Python and the Holy Grail (1975) - Starring: Terry Gilliam (dir.), Graham Chapman, John Cleese 8.151359870088081
    145 - Yôjinbô (1961) - Starring: Akira Kurosawa (dir.), Toshirô Mifune, Eijirô Tôno 8.15020895776589
    146 - The Treasure of the Sierra Madre (1948) - Starring: John Huston (dir.), Humphrey Bogart, Walter Huston 8.149389939725182
    147 - Shutter Island (2010) - Starring: Martin Scorsese (dir.), Leonardo DiCaprio, Emily Mortimer 8.144918652934273
    148 - Jurassic Park (1993) - Starring: Steven Spielberg (dir.), Sam Neill, Laura Dern 8.144291306618683
    149 - Rashômon (1950) - Starring: Akira Kurosawa (dir.), Toshirô Mifune, Machiko Kyô 8.142961326536549
    150 - The Great Escape (1963) - Starring: John Sturges (dir.), Steve McQueen, James Garner 8.14291288570207
    151 - Kill Bill: Vol 1 (2003) - Starring: Quentin Tarantino (dir.), Uma Thurman, David Carradine 8.137732009543095
    152 - No Country for Old Men (2007) - Starring: Ethan Coen (dir.), Tommy Lee Jones, Javier Bardem 8.136635031552812
    153 - Finding Nemo (2003) - Starring: Andrew Stanton (dir.), Albert Brooks, Ellen DeGeneres 8.131562112393567
    154 - The Elephant Man (1980) - Starring: David Lynch (dir.), Anthony Hopkins, John Hurt 8.13084518017412
    155 - Chinatown (1974) - Starring: Roman Polanski (dir.), Jack Nicholson, Faye Dunaway 8.129825216221422
    156 - Raging Bull (1980) - Starring: Martin Scorsese (dir.), Robert De Niro, Cathy Moriarty 8.129506436940298
    157 - The Thing (1982) - Starring: John Carpenter (dir.), Kurt Russell, Wilford Brimley 8.126862656316023
    158 - Gone with the Wind (1939) - Starring: Victor Fleming (dir.), Clark Gable, Vivien Leigh 8.126818190932685
    159 - V for Vendetta (2006) - Starring: James McTeigue (dir.), Hugo Weaving, Natalie Portman 8.125460504023907
    160 - Inside Out (2015) - Starring: Pete Docter (dir.), Amy Poehler, Bill Hader 8.1237563066696
    161 - Lock, Stock and Two Smoking Barrels (1998) - Starring: Guy Ritchie (dir.), Jason Flemyng, Dexter Fletcher 8.122948230325038
    162 - Dial M for Murder (1954) - Starring: Alfred Hitchcock (dir.), Ray Milland, Grace Kelly 8.12042877596097
    163 - El secreto de sus ojos (2009) - Starring: Juan José Campanella (dir.), Ricardo Darín, Soledad Villamil 8.118120799826459
    164 - Hauru no ugoku shiro (2004) - Starring: Hayao Miyazaki (dir.), Chieko Baishô, Takuya Kimura 8.114797225215755
    165 - The Bridge on the River Kwai (1957) - Starring: David Lean (dir.), William Holden, Alec Guinness 8.114308791093633
    166 - Three Billboards Outside Ebbing, Missouri (2017) - Starring: Martin McDonagh (dir.), Frances McDormand, Woody Harrelson 8.1113441451584
    167 - Trainspotting (1996) - Starring: Danny Boyle (dir.), Ewan McGregor, Ewen Bremner 8.111230918941775
    168 - Warrior (2011) - Starring: Gavin O'Connor (dir.), Tom Hardy, Nick Nolte 8.104615859144351
    169 - Gran Torino (2008) - Starring: Clint Eastwood (dir.), Clint Eastwood, Bee Vang 8.10450023279596
    170 - Fargo (1996) - Starring: Joel Coen (dir.), William H. Macy, Frances McDormand 8.1032902292136
    171 - Everything Everywhere All at Once (2022) - Starring: Dan Kwan (dir.), Michelle Yeoh, Stephanie Hsu 8.10166534887231
    172 - Prisoners (2013) - Starring: Denis Villeneuve (dir.), Hugh Jackman, Jake Gyllenhaal 8.09717636562441
    173 - Tonari no Totoro (1988) - Starring: Hayao Miyazaki (dir.), Hitoshi Takagi, Noriko Hidaka 8.094754481159763
    174 - Million Dollar Baby (2004) - Starring: Clint Eastwood (dir.), Hilary Swank, Clint Eastwood 8.089875971442442
    175 - The Gold Rush (1925) - Starring: Charles Chaplin (dir.), Charles Chaplin, Mack Swain 8.087995860190874
    176 - Catch Me If You Can (2002) - Starring: Steven Spielberg (dir.), Leonardo DiCaprio, Tom Hanks 8.087353353402815
    177 - Blade Runner (1982) - Starring: Ridley Scott (dir.), Harrison Ford, Rutger Hauer 8.086025226472353
    178 - On the Waterfront (1954) - Starring: Elia Kazan (dir.), Marlon Brando, Karl Malden 8.084645780611714
    179 - Bacheha-Ye aseman (1997) - Starring: Majid Majidi (dir.), Mohammad Amir Naji, Amir Farrokh Hashemian 8.082748367007026
    180 - The Third Man (1949) - Starring: Carol Reed (dir.), Orson Welles, Joseph Cotten 8.081904366138408
    181 - Ben-Hur (1959) - Starring: William Wyler (dir.), Charlton Heston, Jack Hawkins 8.079293131043624
    182 - Before Sunrise (1995) - Starring: Richard Linklater (dir.), Ethan Hawke, Julie Delpy 8.078657052156387
    183 - Smultronstället (1957) - Starring: Ingmar Bergman (dir.), Victor Sjöström, Bibi Andersson 8.07848544227412
    184 - 12 Years a Slave (2013) - Starring: Steve McQueen (dir.), Chiwetel Ejiofor, Michael Kenneth Williams 8.078338760644112
    185 - The General (1926) - Starring: Clyde Bruckman (dir.), Buster Keaton, Marion Mack 8.078091311617884
    186 - Gone Girl (2014) - Starring: David Fincher (dir.), Ben Affleck, Rosamund Pike 8.077382975665294
    187 - Harry Potter and the Deathly Hallows: Part 2 (2011) - Starring: David Yates (dir.), Daniel Radcliffe, Emma Watson 8.076883849442726
    188 - The Deer Hunter (1978) - Starring: Michael Cimino (dir.), Robert De Niro, Christopher Walken 8.075371879489818
    189 - In the Name of the Father (1993) - Starring: Jim Sheridan (dir.), Daniel Day-Lewis, Pete Postlethwaite 8.074545460226874
    190 - The Grand Budapest Hotel (2014) - Starring: Wes Anderson (dir.), Ralph Fiennes, F. Murray Abraham 8.07323051797225
    191 - Le salaire de la peur (1953) - Starring: Henri-Georges Clouzot (dir.), Yves Montand, Charles Vanel 8.07177283092303
    192 - Mr Smith Goes to Washington (1939) - Starring: Frank Capra (dir.), James Stewart, Jean Arthur 8.071729105391539
    193 - Barry Lyndon (1975) - Starring: Stanley Kubrick (dir.), Ryan O'Neal, Marisa Berenson 8.07074513062646
    194 - Sherlock Jr (1924) - Starring: Buster Keaton (dir.), Buster Keaton, Kathryn McGuire 8.069665621091762
    195 - Salinui chueok (2003) - Starring: Bong Joon Ho (dir.), Song Kang-ho, Kim Sang-kyung 8.068519357593507
    196 - Hacksaw Ridge (2016) - Starring: Mel Gibson (dir.), Andrew Garfield, Sam Worthington 8.068215869204318
    197 - Klaus (2019) - Starring: Sergio Pablos (dir.), Jason Schwartzman, J.K. Simmons 8.0679803020642
    198 - Det sjunde inseglet (1957) - Starring: Ingmar Bergman (dir.), Max von Sydow, Gunnar Björnstrand 8.06656511358373
    199 - Relatos salvajes (2014) - Starring: Damián Szifron (dir.), Darío Grandinetti, María Marull 8.06628007171741
    200 - Room (2015) - Starring: Lenny Abrahamson (dir.), Brie Larson, Jacob Tremblay 8.06584636300777
    201 - Mad Max: Fury Road (2015) - Starring: George Miller (dir.), Tom Hardy, Charlize Theron 8.062534576505719
    202 - How to Train Your Dragon (2010) - Starring: Dean DeBlois (dir.), Jay Baruchel, Gerard Butler 8.061861544792881
    203 - The Big Lebowski (1998) - Starring: Joel Coen (dir.), Jeff Bridges, John Goodman 8.06182991636748
    204 - Mary and Max (2009) - Starring: Adam Elliot (dir.), Toni Collette, Philip Seymour Hoffman 8.060920670636838
    205 - Monsters, Inc (2001) - Starring: Pete Docter (dir.), Billy Crystal, John Goodman 8.059274202372771
    206 - Jaws (1975) - Starring: Steven Spielberg (dir.), Roy Scheider, Robert Shaw 8.05859190665953
    207 - La passion de Jeanne d'Arc (1928) - Starring: Carl Theodor Dreyer (dir.), Maria Falconetti, Eugene Silvain 8.056323567894106
    208 - Tôkyô monogatari (1953) - Starring: Yasujirô Ozu (dir.), Chishû Ryû, Chieko Higashiyama 8.055086171549508
    209 - Dead Poets Society (1989) - Starring: Peter Weir (dir.), Robin Williams, Robert Sean Leonard 8.053773031789591
    210 - Hotel Rwanda (2004) - Starring: Terry George (dir.), Don Cheadle, Sophie Okonedo 8.052482200930354
    211 - Rocky (1976) - Starring: John G. Avildsen (dir.), Sylvester Stallone, Talia Shire 8.047690016343408
    212 - Platoon (1986) - Starring: Oliver Stone (dir.), Charlie Sheen, Tom Berenger 8.046990368208261
    213 - Ford v Ferrari (2019) - Starring: James Mangold (dir.), Matt Damon, Christian Bale 8.046463885406821
    214 - Pather Panchali (1955) - Starring: Satyajit Ray (dir.), Kanu Bannerjee, Karuna Bannerjee 8.045109956146478
    215 - Stand by Me (1986) - Starring: Rob Reiner (dir.), Wil Wheaton, River Phoenix 8.042170923120207
    216 - The Terminator (1984) - Starring: James Cameron (dir.), Arnold Schwarzenegger, Linda Hamilton 8.041602094924668
    217 - Spotlight (2015) - Starring: Tom McCarthy (dir.), Mark Ruffalo, Michael Keaton 8.03966265641161
    218 - Rush (2013) - Starring: Ron Howard (dir.), Daniel Brühl, Chris Hemsworth 8.038643203574669
    219 - Network (1976) - Starring: Sidney Lumet (dir.), Faye Dunaway, William Holden 8.037279388524656
    220 - Logan (2017) - Starring: James Mangold (dir.), Hugh Jackman, Patrick Stewart 8.036857503198219
    221 - Into the Wild (2007) - Starring: Sean Penn (dir.), Emile Hirsch, Vince Vaughn 8.036122605926156
    222 - The Wizard of Oz (1939) - Starring: Victor Fleming (dir.), Judy Garland, Frank Morgan 8.03548840022015
    223 - Ratatouille (2007) - Starring: Brad Bird (dir.), Brad Garrett, Lou Romano 8.03442302184599
    224 - Groundhog Day (1993) - Starring: Harold Ramis (dir.), Bill Murray, Andie MacDowell 8.032191803014308
    225 - Before Sunset (2004) - Starring: Richard Linklater (dir.), Ethan Hawke, Julie Delpy 8.029622194384197
    226 - The Exorcist (1973) - Starring: William Friedkin (dir.), Ellen Burstyn, Max von Sydow 8.02802955037334
    227 - The Best Years of Our Lives (1946) - Starring: William Wyler (dir.), Myrna Loy, Dana Andrews 8.027243986738085
    228 - The Incredibles (2004) - Starring: Brad Bird (dir.), Craig T. Nelson, Samuel L. Jackson 8.025664984778295
    229 - To Be or Not to Be (1942) - Starring: Ernst Lubitsch (dir.), Carole Lombard, Jack Benny 8.024400143786794
    230 - La battaglia di Algeri (1966) - Starring: Gillo Pontecorvo (dir.), Brahim Hadjadj, Jean Martin 8.023296797621018
    231 - The Grapes of Wrath (1940) - Starring: John Ford (dir.), Henry Fonda, Jane Darwell 8.023271912718382
    232 - Rebecca (1940) - Starring: Alfred Hitchcock (dir.), Laurence Olivier, Joan Fontaine 8.023024244644526
    233 - Hachi: A Dog's Tale (2009) - Starring: Lasse Hallström (dir.), Richard Gere, Joan Allen 8.022206142261863
    234 - Cool Hand Luke (1967) - Starring: Stuart Rosenberg (dir.), Paul Newman, George Kennedy 8.02194665518381
    235 - Amores perros (2000) - Starring: Alejandro G. Iñárritu (dir.), Emilio Echevarría, Gael García Bernal 8.021393939621179
    236 - Pirates of the Caribbean: The Curse of the Black Pearl (2003) - Starring: Gore Verbinski (dir.), Johnny Depp, Geoffrey Rush 8.020267155441346
    237 - La haine (1995) - Starring: Mathieu Kassovitz (dir.), Vincent Cassel, Hubert Koundé 8.018424321492349
    238 - Les quatre cents coups (1959) - Starring: François Truffaut (dir.), Jean-Pierre Léaud, Albert Rémy 8.016941111210334
    239 - Persona (1966) - Starring: Ingmar Bergman (dir.), Bibi Andersson, Liv Ullmann 8.01608646035709
    240 - Babam ve Oglum (2005) - Starring: Cagan Irmak (dir.), Çetin Tekindor, Fikret Kuskan 8.015999392715981
    241 - It Happened One Night (1934) - Starring: Frank Capra (dir.), Clark Gable, Claudette Colbert 8.01523432501558
    242 - Life of Brian (1979) - Starring: Terry Jones (dir.), Graham Chapman, John Cleese 8.013571095478623
    243 - The Sound of Music (1965) - Starring: Robert Wise (dir.), Julie Andrews, Christopher Plummer 8.013529957311984
    244 - Ah-ga-ssi (2016) - Starring: Park Chan-wook (dir.), Kim Min-hee, Ha Jung-woo 8.011039354923366
    245 - Dersu Uzala (1975) - Starring: Akira Kurosawa (dir.), Maksim Munzuk, Yuriy Solomin 8.010992855333495
    246 - Jai Bhim (2021) - Starring: T.J. Gnanavel (dir.), Suriya, Lijo Mol Jose 8.006715671663374
    247 - Aladdin (1992) - Starring: Ron Clements (dir.), Scott Weinger, Robin Williams 8.006412909670424
    248 - Gandhi (1982) - Starring: Richard Attenborough (dir.), Ben Kingsley, John Gielgud 8.006294658015946
    249 - The Help (2011) - Starring: Tate Taylor (dir.), Emma Stone, Viola Davis 8.005033850723157
    250 - The Iron Giant (1999) - Starring: Brad Bird (dir.), Eli Marienthal, Harry Connick Jr. 8.00227178495195
    


```python

```


```python

```


```python

```
