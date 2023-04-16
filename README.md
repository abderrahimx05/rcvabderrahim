![pexels-pixabay-209956](https://user-images.githubusercontent.com/75130312/205675229-00ce6fa3-3478-474b-a39a-11ee81885ee9.jpg)
![ship](https://user-images.githubusercontent.com/75130312/206386722-593a7180-bec4-4f3d-b41c-3b3726a6c706.png)
![pngegg](https://user-images.githubusercontent.com/75130312/206395228-c49b2f15-887f-4236-8118-2bdce01cfdc9.png)
view-source:https://technext.github.io/tour/?_ga=2.186626473.1272154040.1670538450-801814825.1670538450
<form>
    <div class="form-group">
        <label for="inputEmail">Email</label>
        <input type="email" class="form-control" id="inputEmail" placeholder="Email">
    </div>
    <div class="form-group">
        <label for="inputPassword">Password</label>
        <input type="password" class="form-control" id="inputPassword" placeholder="Password">
    </div>
    <div class="form-group">
        <label class="form-check-label"><input type="checkbox"> Remember me</label>
    </div>
    <button type="submit" class="btn btn-primary">Sign in</button>
</form>


 <div class="container contact-form">
            <div class="contact-image">
                <img src="https://image.ibb.co/kUagtU/rocket_contact.png" alt="rocket_contact"/>
            </div>
            <form method="post">
                <h3>Drop Us a Message</h3>
               <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <input type="text" name="txtName" class="form-control" placeholder="Your Name *" value="" />
                        </div>
                        <div class="form-group">
                            <input type="text" name="txtEmail" class="form-control" placeholder="Your Email *" value="" />
                        </div>
                        <div class="form-group">
                            <input type="text" name="txtPhone" class="form-control" placeholder="Your Phone Number *" value="" />
                        </div>
                        <div class="form-group">
                            <input type="submit" name="btnSubmit" class="btnContact" value="Send Message" />
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <textarea name="txtMsg" class="form-control" placeholder="Your Message *" style="width: 100%; height: 150px;"></textarea>
                        </div>
                    </div>
                </div>
            </form>
            
            https://bootsnipp.com/snippets/7nmOW
    
    <style type="text/css">
    body{
    background: -webkit-linear-gradient(left, #0072ff, #00c6ff);
}
.contact-form{
    background: #fff;
    margin-top: 10%;
    margin-bottom: 5%;
    width: 70%;
}
.contact-form .form-control{
    border-radius:1rem;
}
.contact-image{
    text-align: center;
}
.contact-image img{
    border-radius: 6rem;
    width: 11%;
    margin-top: -3%;
    transform: rotate(29deg);
}
.contact-form form{
    padding: 14%;
}
.contact-form form .row{
    margin-bottom: -7%;
}
.contact-form h3{
    margin-bottom: 8%;
    margin-top: -10%;
    text-align: center;
    color: #0062cc;
}
.contact-form .btnContact {
    width: 50%;
    border: none;
    border-radius: 1rem;
    padding: 1.5%;
    background: #dc3545;
    font-weight: 600;
    color: #fff;
    cursor: pointer;
}
.btnContactSubmit
{
    width: 50%;
    border-radius: 1rem;
    padding: 1.5%;
    color: #fff;
    background-color: #0062cc;
    border: none;
    cursor: pointer;
}    </style>


//log page 
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\HttpFoundation\Request;
use App\Entity\Client;
use App\Entity\Reservation;

class LoginController extends AbstractController
{
    #[Route('/login', name: 'app_login')]
    public function index(): Response
    {
        return $this->render('login/index.html.twig', [
            'controller_name' => 'LoginController',
        ]);
    }

    #[Route('/login/check', name: 'app_login_check')]
    public function check(Request $request)
    {
        // récupérer les données envoyées par le formulaire de connexion
        $email = $request->request->get('becker1');
        $password = $request->request->get('1234');

        // récupérer le client depuis la base de données en utilisant l'email et le mot de passe
        $client = $this->getDoctrine()
            ->getRepository(Client::class)
            ->findOneBy([
                'email' => $email,
                'password' => $password,
            ]);

        // si le client n'existe pas, afficher un message d'erreur et rediriger vers la page de connexion
        if (!$client) {
            return $this->render('login/index.html.twig', [
                'error' => 'Invalid login credentials',
            ]);
        }

        // si le client existe, créer une session pour lui et rediriger vers la page d'accueil
        $session = $request->getSession();
        $session->set('user_id', $client->getId());

        return $this->redirectToRoute('home');
    }

    #[Route('/logout', name: 'app_logout')]
    public function logout(Request $request)
    {
        // supprimer la session de l'utilisateur et rediriger vers la page de connexion
        $session = $request->getSession();
        $session->remove('user_id');

        return $this->redirectToRoute('app_login');
    }

    #[Route('/reservation/{id}', name: 'app_reservation_new')]
    public function reservation($id): Response
    {
        // récupérer le client depuis la base de données en utilisant l'ID
        $client = $this->getDoctrine()
            ->getRepository(Client::class)
            ->find($id);

        // si le client n'existe pas, afficher une erreur 404
        if (!$client) {
            throw $this->createNotFoundException('Client not found');
        }

        // récupérer toutes les réservations du client et les afficher
        $reservations = $client->getReservations();

        return $this->render('reservation/index.html.twig', [
            'reservations' => $reservations,
        ]);
    }
}
