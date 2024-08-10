# Pro2024
final code mock
Event Management Code Start- 

POM=
project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> 
          
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.promnieotech</groupId>
    <artifactId>event-management-system</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Event Management System</name>
    <description>Event Management System using Spring Boot</description>

    <properties>
        <java.version>17</java.version>
        <spring-boot.version>3.0.0</spring-boot.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
     <dependencies>
        <!-- Spring Boot Starter Web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Spring Boot Starter Data JPA -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- Spring Boot Starter Security (optional) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <!-- H2 Database (for development) -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- Swagger (for API documentation) -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>${spring-boot.version}</version>
            </plugin>
        </plugins>
    </build>
</project>

Event Management model 
Event Model=package com.promineotech.eventmanagement.model;

import java.time.LocalDateTime;
import java.util.Set;

import org.springframework.security.core.userdetails.User;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.JoinTable;
import jakarta.persistence.ManyToMany;
import jakarta.persistence.ManyToOne;

@SuppressWarnings("unused")
@Entity
public class Event {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    @ManyToOne Ma
    @JoinColumn(name = "user_id")
    private User creator;

    @ManyToMany
    @JoinTable(name = "event_user",
               joinColumns = @JoinColumn(name = "event_id"),
               inverseJoinColumns = @JoinColumn(name = "user_id"))
    private Set<User> attendees;
    }

    Event Management model
    Registration
    package com.promineotech.eventmanagement.model;

import java.time.LocalDateTime;

import org.springframework.boot.autoconfigure.security.SecurityProperties.User;

public class Registration {

	public Registration() {
		
	}

	public void setEvent(Event event) {
		// TODO Auto-generated method stub
		
	}

	public void setRegistrationDate(LocalDateTime now) {
		// TODO Auto-generated method stub
		
	}

	public void setUser(User user) {
		// TODO Auto-generated method stub
		
	}

}

   Event Management model
    User Model- 
com.promineotech.eventmanagement.model;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;

@Entity
public class User { 
	
    
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String email;

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}

package com.promineotech.eventmanagement.repository;

import com.promineotech.eventmanagement.model.Event;
import org.springframework.data.jpa.repository.JpaRepository;

public interface EventRepository extends JpaRepository<Event, Long> {
}


package com.promineotech.eventmanagement.repository;

import java.util.List;

import com.promineotech.eventmanagement.model.Event;
import com.promineotech.eventmanagement.model.Registration;

public interface RegistrationRepository {

	Registration save(Registration registration);

	List<Registration> findByEvent(Event event);

	void deleteAll(List<Registration> registrations);

}

package com.promineotech.eventmanagement.repository;

import org.springframework.boot.autoconfigure.security.SecurityProperties.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}




package com.promnieotech.eventmanagement.config;

public enum AuthenticationManagerBuilder {
	;

	Object inMemoryAuthentication() {
		// TODO Auto-generated method stub
		return null;
	}

}
package com.promnieotech.eventmanagement.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.core.userdetails.User;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @SuppressWarnings("deprecation")
	protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .requestMatchers("/api/users/**", "/api/events/**").authenticated()
                .and()
            .httpBasic();
    }

    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
       
           
      
    }
}

package com.promnieotech.eventmanagement.config;

public class SwaggerConfig {

	public SwaggerConfig() {
		// TODO Auto-generated constructor stub
	}

}

package com.promnieotech.eventmanagement.controller;

import com.promineotech.eventmanagement.model.Event;
import com.promineotech.eventmanagement.model.User;
import com.promineotech.eventmanagement.service.EventService;
import com.promineotech.eventmanagement.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.Set;

@RestController
@RequestMapping("/api/events")
public class EventController {

    @Autowired
    private EventService eventService;

    @Autowired
    private UserService userService;

    // Create Event
    @PostMapping
    public Event createEvent(@RequestBody Event event) {
        return eventService.saveEvent(event);
    }

    // Read Event
    @GetMapping("/{id}")
    public Event getEvent(@PathVariable Long id) {
        return eventService.getEventById(id);
    }

    // Update Event
    @PutMapping("/{id}")
    public Event updateEvent(@PathVariable Long id, @RequestBody Event event) {
        return eventService.updateEvent(id, event);
    }

    // Delete Event
    @DeleteMapping("/{id}")
    public void deleteEvent(@PathVariable Long id) {
        eventService.deleteEvent(id);
    }

    // Register User for Event
    @PostMapping("/{eventId}/register/{userId}")
    public void registerUser(@PathVariable Long eventId, @PathVariable Long userId) {
        eventService.registerUser(eventId, userId);
    }

    // Get All Registrations for Event
    @GetMapping("/{eventId}/registrations")
    public Set<User> getRegistrations(@PathVariable Long eventId) {
        return eventService.getRegistrations(eventId);
    }

    // Unregister User from Event
    @DeleteMapping("/{eventId}/register/{userId}")
    public void unregisterUser(@PathVariable Long eventId, @PathVariable Long userId) {
        eventService.unregisterUser(eventId, userId);
    }
}    

package com.promnieotech.eventmanagement.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import com.promineotech.eventmanagement.model.Registration;

import java.util.List;

@RestController
@RequestMapping("/api/events")
public class RegistrationController {

	 @Autowired
	    private RegistrationService registrationService;

	    @PostMapping("/{eventId}/register/{userId}")
	    public Registration registerUserForEvent(@PathVariable Long eventId, @PathVariable Long userId) {
	        return registrationService.registerUserForEvent(eventId, userId);
	    }

	    @GetMapping("/{eventId}/registrations")
	    public List<Registration> getAllRegistrationsForEvent(@PathVariable Long eventId) {
	        return registrationService.getRegistrationsByEvent(eventId);
	    }

	    @DeleteMapping("/{eventId}/register/{userId}")
	    public void unregisterUserFromEvent(@PathVariable Long eventId, @PathVariable Long userId) {
	        registrationService.unregisterUserFromEvent(eventId, userId);
	    }
	}

 package com.promnieotech.eventmanagement.controller;

import com.promnieotech.eventmanagement.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/users")
public class Usercontroller<User> {

    @Autowired
    private UserService userService;

    // Create User
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.saveUser(user);
    }

    // Read User
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.getUserById(id);
    }

    // Update User
    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.updateUser(id, user);
    }

    // Delete User
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}

/**
 * 
 */
package com.promnieotech.eventmanagement.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
/**
 * 
 */
public class EventService {


    @Autowired
    private EventRepository eventRepository;

    @Autowired
    private UserRepository userRepository;

    public Event saveEvent(Event event) {
        return eventRepository.save(event);
    }

    public Event getEventById(Long id) {
        return eventRepository.findById(id).orElse(null);
    }

    public Event updateEvent(Long id, Event event) {
        event.setId(id);
        return eventRepository.save(event);
    }

    public void deleteEvent(Long id) {
        eventRepository.deleteById(id);
    }

    public void registerUser(Long eventId, Long userId) {
        Event event = getEventById(eventId);
        User user = userRepository.findById(userId).orElse(null);
        if (event != null && user != null) {
            event.getAttendees().add(user);
            eventRepository.save(event);
        }
    }

    public Set<User> getRegistrations(Long eventId) {
        Event event = getEventById(eventId);
        return event != null ? event.getAttendees() : Collections.emptySet();
    }

    public void unregisterUser(Long eventId, Long userId) {
        Event event = getEventById(eventId);
        User user = userRepository.findById(userId).orElse(null);
        if (event != null && user != null) {
            event.getAttendees().remove(user);
            eventRepository.save(event);
        }
    }
}
package com.promnieotech.eventmanagement.service;

import com.promineotech.eventmanagement.model.Registration;

public interface RegistrationRepository {

	Registration save(Registration registration);

}

package com.promnieotech.eventmanagement.service;

import java.time.LocalDateTime;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.User;
import org.springframework.stereotype.Service;

import com.promineotech.eventmanagement.model.Event;
import com.promineotech.eventmanagement.model.Registration;
import com.promineotech.eventmanagement.repository.EventRepository;
import com.promineotech.eventmanagement.repository.RegistrationRepository;
import com.promineotech.eventmanagement.repository.UserRepository;

@Service
public class RegistrationService {

    @Autowired
    private RegistrationRepository registrationRepository;

    @Autowired
    private EventRepository eventRepository;

    @Autowired
    private UserRepository userRepository;

    public Registration registerUserForEvent(Long eventId, Long userId) {
        // Retrieve the Event by ID
        Event event = eventRepository.findById(eventId).orElse(null);

        // Retrieve the User by ID
        org.springframework.boot.autoconfigure.security.SecurityProperties.User user = userRepository.findById(userId).orElse(null);

        if (event != null && user != null) {
            Registration registration = new Registration();
            registration.setEvent(event);
            registration.setUser(user);
            registration.setRegistrationDate(LocalDateTime.now());

            return registrationRepository.save (registration);
        } else {
            // Handle the case where either event or user is not found
            throw new RuntimeException("Event or User not found");
        }
    }

    public List<Registration> getRegistrationsByEvent(Long eventId) {
        // Retrieve the Event by ID
        Event event = eventRepository.findById(eventId).orElse(null);
        return event != null ? registrationRepository.findByEvent(event) : null;
    }

    public void unregisterUserFromEvent(Long eventId, Long userId) {
        // Retrieve the Event by ID
        Event event = eventRepository.findById(eventId).orElse(null);
        org.springframework.boot.autoconfigure.security.SecurityProperties.User user = userRepository.findById(userId).orElse(null);

        if (event != null && user != null) {
            List<Registration> registrations = registrationRepository.findByEvent(event);
            registrations.removeIf(reg -> reg.getUser().equals(user));
            registrationRepository.deleteAll(registrations);
        } else {
            // Handle the case where either event or user is not found
            throw new RuntimeException("Event or User not found");
        }
    }
}

package com.promnieotech.eventmanagement.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.security.SecurityProperties.User;
import org.springframework.stereotype.Service;

import com.promineotech.eventmanagement.repository.UserRepository; // 

@Service 
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public User saveUser(User user) {
        return userRepository.save(user);
    }

    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }


    
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }

	public <User> User updateUser(Long id, User user) {
		// TODO Auto-generated method stub
		return null;
	}
}



