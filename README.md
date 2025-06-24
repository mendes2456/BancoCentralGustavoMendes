# BancoCentralGustavoMendes
// Projeto: BankGuard - Verificação Inteligente de Acesso Bancário
// Linguagem: Java + Spring Boot
// Banco de Dados: MySQL

// Estrutura inicial do projeto (microserviço de autenticação + microserviço de verificação)
// Este é o microserviço de autenticação (auth-service)

// AuthServiceApplication.java
package com.bankguard.authservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AuthServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(AuthServiceApplication.class, args);
    }
}

// User.java (Model)
package com.bankguard.authservice.model;

import jakarta.persistence.*;

@Entity
@Table(name = "usuarios")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nome;
    private String email;
    private String senha;
    private String telefone;

    // Getters e Setters
}

// UserRepository.java
package com.bankguard.authservice.repository;

import com.bankguard.authservice.model.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    User findByEmail(String email);
}

// AuthController.java
package com.bankguard.authservice.controller;

import com.bankguard.authservice.model.User;
import com.bankguard.authservice.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/auth")
public class AuthController {

    @Autowired
    private UserRepository userRepository;

    @PostMapping("/register")
    public User register(@RequestBody User user) {
        return userRepository.save(user);
    }

    @PostMapping("/login")
    public String login(@RequestBody User user) {
        User found = userRepository.findByEmail(user.getEmail());
        if (found != null && found.getSenha().equals(user.getSenha())) {
            return "Login bem-sucedido!";
        }
        return "Credenciais inválidas.";
    }
}

// application.properties
spring.datasource.url=jdbc:mysql://localhost:3306/bankguard
spring.datasource.username=root
spring.datasource.password=senha
spring.jpa.hibernate.ddl-auto=update
server.port=8081
