# BuilderVsObjectMoter

Scope: Use builder pattern instead of object mother.


## ObjectMother 
ObjectMother starts with the factory pattern, by delivering prefabricated test-ready objects via a simple method call. 

Some reasons to use ObjectMother:
- Reduce code duplication in tests, increasing test maintainability
- Make test objects super-easily accessible, encouraging developers to write more tests.
- Every test runs with fresh data.
- Tests always clean up after themselves.

``` 
public class UsersOM {

    public static User aRegularUser() {
        return new User("Eugeniu", "Cararus", "ROLE_USER");
    }

	public static User aPriviledgedUser() {
        return new User("Martin", "Bosh", "ROLE_SUPER_USER");
    }

    // other factory methods

}

public class RoleServiceTest{
	
	@Test
	public void shouldExecuteSuFromRegularUser(){
		User regularUser = UsersOM.aRegularUser();
		User superUser = UsersOM.aPriviledgedUser();
		// The SUT.
	}	
}

``` 
The problem whith this kind of aproache commes when the User object is complex and combination of users to be created for different scenatios is big, by keeping creating Object Mother the menatinance and cleanning of OMother is bigger chalenge than the value which it brings for tests.

## Test Data Builder
Test Data builder starts with [Builder pattern](https://cararuseugeniu.blogspot.co.uk/p/design-patterns-in-images.html) to create objects in Unit Tests. 
The TestDataBuilder pattern allows tests to specify only those parts of the objects that need to vary and use sensible defaults for those that are not relevant to the test.

``` 
public class UserBuilder {

    public static final String DEFAULT_FIRSTNAME = "Eugeniu";
    public static final String DEFAULT_SURNAME = "Cararus";
    public static final String DEFAULT_ROLE = "ROLE_USER";

    private String firstname = DEFAULT_FIRSTNAME;
    private String surname = DEFAULT_SURNAME;
    private String role = DEFAULT_ROLE;
    
    private UserBuilder() {
    }

    public static UserBuilder aUser() {
        return new UserBuilder();
    }

    public UserBuilder withFirstname(String firstname) {
        this.firstname = firstname;
        return this;
    }

    public UserBuilder withSurname(String surname) {
        this.surname = surname;
        return this;
    }

    public UserBuilder withNoSurname() {
        this.surname = null;
        return this;
    }

    public UserBuilder inRegularUser() {
        this.role = "ROLE_USER";
        return this;
    }

    public UserBuilder inPriviledgedUser() {
        this.role = "ROLE_ADMIN";
        return this;
    }

    public UserBuilder inRole(String role) {
        this.role = role;
        return this;
    }

    public User build() {
        return new User(firstname, surname, role);
    }
}

public class RoleServiceTest{
	
	@Test
	public void shouldExecuteSuFromRegularUser(){
		User regularUser = UserBuilder.build();
		User superUser = UserBuilder.withFirstname("Martin").withSurname("Bosh").inPriviledgedUser().build();
		// The SUT.
	}	
}
``` 

Useful links: 
http://c2.com/cgi/wiki?ObjectMother
http://c2.com/cgi/wiki?TestDataBuilder

## DISCLAIMER:
Purpose of project is only educational.
This project should not be used for any commercial purpose.
This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.
http://creativecommons.org/licenses/by-nc-sa/4.0/.

## Author:
Eugeniu Cararus
cararuseugeniu@gmail.com
