# PHP-application-ECR
Hosting a PHP application on Amazon ECR Repository
# Use an official PHP runtime as a parent image
FROM php:7.4-apache

# Set the working directory in the container
WORKDIR /var/www/html

# Copy the current directory contents into the container at /var/www/html
COPY . /var/www/html

# Install additional PHP extensions and tools
RUN apt-get update && \
    apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libzip-dev \
        unzip \
        git \
    && docker-php-ext-install -j$(nproc) gd mysqli pdo_mysql zip

# Enable Apache modules
RUN a2enmod rewrite

# Restart Apache to apply changes
RUN service apache2 restart

~                                                                                                                                                
~                                                                                                                                                
"Dockerfile" 26L, 631B                                                                                                         26,0-1        All
Dockerfile Explanation:
*************************************
FROM php:7.4-apache: This line sets the base image for your Dockerfile. It includes PHP 7.4 and Apache already configured.

WORKDIR /var/www/html: Sets the working directory inside the container where Apache will serve files from.

COPY . /var/www/html: Copies all files from your current directory on your host machine into the /var/www/html directory inside the container.

RUN docker-php-ext-install mysqli pdo pdo_mysql: Installs PHP extensions needed for database connectivity. Adjust this line as per your application's requirements.

RUN a2enmod rewrite: Enables the Apache mod_rewrite module, which is often needed for PHP applications that use URL rewriting.

EXPOSE 80: Informs Docker that the container listens on port 80 at runtime. This does not actually publish the port, but it is a good practice to include it for documentation purposes.

CMD ["apache2-foreground"]: Specifies the command to run when the container starts. Here, it starts the Apache web server.
Build the Docker Image
After creating the Dockerfile, build the Docker image using the following command. Make sure you are in the directory where your Dockerfile is located.
use command :  docker build -t my-php-app .
Run the Docker Container
Once the image is built, you can run the Docker container using:
docker run -p 8080:80 my-php-app
This command maps port 8080 on your host machine to port 80 inside the Docker container where Apache is running.
Access Your PHP Application
Open a web browser and go to http://localhost:8080 to view your PHP application running inside the Docker container.
create a repository on Amazon
make sure you ahve python AWS cli installed on the system
run the AWS configure
make sure to run the AWS and Docker login
this will store the docker credentials on the local system in config.json file on home directory
use the push commands available on ECR and make chnages as required on the commands to push the image with required tag name.
Build your Docker image using the following command.
docker build -t myrepo .
After the build completes, tag your image so you can push the image to this repository:
docker tag myrepo:latest public.ecr.aws/d8h8k0u3/myrepo:latest
Run the following command to push this image to your newly created AWS repository:
docker push public.ecr.aws/d8h8k0u3/myrepo:latest

Once your Docker image is in ECR, you can use it in your ECS (Elastic Container Service) tasks or anywhere else Docker images are used within AWS.
By following these steps, you can effectively move your Docker image containing your PHP application from your local environment to Amazon ECR for production or deployment purposes. Adjustments may be needed based on your specific AWS setup and permissions.


