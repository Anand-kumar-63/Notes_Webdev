# What is a virtual machine 
# what is hypervisor 

Steps-
- Gave name to my virtual machine.  
- you selected the operating system => that is ubuntu or amazon Linux or Linux or mac or Red Hat or Fedora or any other OS
- Selected the OS version 
- Select the instance type
- Created the keypair and set it 
- Added a new security group and opened port 3000 and 22 
- Add 16gb storage 
# Deploy frontend 
- You only need dist file of the react application 
- Only basic html , css  , js applicatons and react applications that can be converted into html , css and js..
- This approach will not work for frameworks that uses server side Rendering....like nextjs , angular 
## local hosting without npm run dev you can serve your Dist file
 -  you have to do  npm install -g serve 
 -  and then go to dist folder and run the command serve - it will server you directory on a port so that other people can  access it - it will serve your html css js file on a port.. exposes those files over a port 

#step1 
![[Pasted image 20251119001816.png]]

![[Pasted image 20251119002112.png]]

 
![[Pasted image 20251119002619.png]]
![[Pasted image 20251119003124.png]]
# Creating an object in AWS store 
![[Pasted image 20251119003919.png]]
- Go to S3 and create a new bucket for you react dist folder and paste your dist files index.html , css and js in that bucket 
![[Pasted image 20251119005001.png]]
![[Pasted image 20251119005027.png]]
![[Pasted image 20251119005048.png]]

**![[Pasted image 20251119005111.png]]**

# connecting to you own domain
![[Pasted image 20251119010425.png]]
