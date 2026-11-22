# Exp 04 Spring-Boot-with-REST-API-and-Hibernate-Integration

## AIM:
To develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it via REST APIs.

## ALGORITHM:
1. Create Spring Boot project with dependencies:</br>
* Spring Web</br>
* Spring Data JPA</br>
* MySQL Driver</br>
2. Configure application.properties with DB connection and JPA settings.</br>
3. Create Movie entity with fields like id, title, genre, rating, and year.</br>
4. Create MovieRepository interface extending JpaRepository.</br>
5. Create MovieController to define REST endpoints for CRUD operations:</br>
* GET /movies</br>
* GET /movies/{id}</br>
* POST /movies</br>
* PUT /movies/{id}</br>
* DELETE /movies/{id}</br>
  

## PROGRAM CODE (Main Files):

### application.properties

```
spring.application.name=experiment4
spring.datasource.url=jdbc:mysql://localhost:3306/movie
spring.datasource.username=root
spring.datasource.password=2096

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### Movie.java

```
package com.example.experiment4.model;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Movie {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String genre;
    private int year;
    private double rating;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public double getRating() {
        return rating;
    }

    public void setRating(double rating) {
        this.rating = rating;
    }

    public String getGenre() {
        return genre;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public int getYear() {
        return year;
    }

    public void setYear(int year) {
        this.year = year;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }
}
```

### MovieRepository.java

```
package com.example.experiment4.repository;

import com.example.experiment4.model.Movie;
import org.springframework.data.jpa.repository.JpaRepository;

public interface MovieRepository extends JpaRepository<Movie, Long> {
}
```

### MovieController.java

```
package com.example.experiment4.controller;

import com.example.experiment4.model.Movie;
import com.example.experiment4.repository.MovieRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/movies")
public class MovieController {

    @Autowired
    private MovieRepository repo;

    @GetMapping
    public List<Movie> getAllMovies() {
        return repo.findAll();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Movie> getMovieById(@PathVariable Long id) {
        return repo.findById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public Movie addMovie(@RequestBody Movie movie) {
        return repo.save(movie);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Movie> updateMovie(
            @PathVariable Long id,
            @RequestBody Movie movieDetails) {

        return repo.findById(id).map(movie -> {
            movie.setTitle(movieDetails.getTitle());
            movie.setGenre(movieDetails.getGenre());
            movie.setYear(movieDetails.getYear());
            movie.setRating(movieDetails.getRating());

            return ResponseEntity.ok(repo.save(movie));
        }).orElse(ResponseEntity.notFound().build());
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteMovie(@PathVariable Long id) {

        return repo.findById(id).map(movie -> {
            repo.delete(movie);
            return ResponseEntity.noContent().<Void>build();
        }).orElse(ResponseEntity.notFound().build());
    }
}
```


### Output:

## POST/movies 

<img width="1917" height="1015" alt="Screenshot 2026-11-22 191711" src="https://github.com/user-attachments/assets/21db3479-093c-432c-91ba-3e9b3c74e668" />


## GET/movies

<img width="1917" height="1025" alt="Screenshot 2026-11-22 191759" src="https://github.com/user-attachments/assets/0c02c274-0a8e-40a0-ad0d-c21d0bc564a8" />


## GET/movies/:id

<img width="1913" height="1017" alt="Screenshot 2026-11-22 191839" src="https://github.com/user-attachments/assets/d1c92dc0-342d-4f37-917d-59be13c60839" />


## PUT/movies/:id

<img width="1917" height="1017" alt="Screenshot 2026-11-22 192022" src="https://github.com/user-attachments/assets/f3f031a0-4169-4815-be38-3c051b2974f7" />



## DELETE/movies

<img width="1518" height="1022" alt="Screenshot 2026-11-22 192244" src="https://github.com/user-attachments/assets/61642917-5712-4f00-908b-9047b2ff5b4f" />

## Result:
Thus, The program to develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it via REST APIs is executed successfully.

