# Farmers Insurance
  Here I will explain little bit my RestAssured library framework.
  My framework is Junit. That's why i used Junit 5. I have used @BeforeAll method to start with that URI.
  I added  @DisplayName() to write the little bit information about my test.It will be easy to understand.
  After that depends on the requirement I add my Api token as String use RestAssured method chaining.
Rest Assured, we use method chaining to achieve readable test.
       given(). -> some more information I want to provide other than request url
            like header , query param , path variable , body
       when()  I send the request GET(retrieve the data) ,POST(for create the data), PUT(for update the data)
       then()  I used to validate the my data
       
       
       public class Nasa {
    @BeforeAll
    public static void init(){
        RestAssured.baseURI="https://api.nasa.gov";
        RestAssured.basePath="/planetary";
    }
    @DisplayName("API Testing 1st approach ")
    @Test
    public void test1() {
        String apiToken = "8R5nVa2YSZbADMr7z4Qv2godTPXe4cO6vnVQcNO5";
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        LocalDate date = LocalDate.now();
        given()
                .log().all()
                .header("Authorization", "Bearer " + apiToken)
                .header("ContentType", "Application/json")
                .queryParam("api_key", "8R5nVa2YSZbADMr7z4Qv2godTPXe4cO6vnVQcNO5")
                .queryParam("date", date.format(dtf))
                .queryParam("hd", "bool")
                .when()
                .get("/apod")
                .then()
                .log().all()
                .statusCode(200);


    }
    @DisplayName("API Testing 2nd approach")
    @Test
    public void test2(){
        String apiToken = "8R5nVa2YSZbADMr7z4Qv2godTPXe4cO6vnVQcNO5";
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        LocalDate date = LocalDate.now();
        Response response=
                given()
                        .log().all()
                        .header("Authorization", "Bearer " + apiToken)
                        .header("ContentType", "Application/json")
                        .queryParam("api_key", "8R5nVa2YSZbADMr7z4Qv2godTPXe4cO6vnVQcNO5")
                        .queryParam("date", date.format(dtf))
                        .queryParam("hd", "bool")
                        .when()
                        .get("/apod")
                        .prettyPeek();
        assertEquals(response.statusCode(),200);}}

       
 I have used 2 way of printing the response
 * prettyPrint -->> I print the body and return String
   -- no more chaining method of Response Object if this used

 * prettyPeek --->> I print the entire response object and return same response object
   including status code, header, body
   since it returns same response object
   we can keep chaining the methods after prettyPeek.

   I have used the JsonPath which is a class that have a lot of methods
   so I have been using to get the body data in different format or data types
   I get this object by calling the method of Response object called .jsonPath() and with some methods such as
   getInit(), getString(),getList() exc.
