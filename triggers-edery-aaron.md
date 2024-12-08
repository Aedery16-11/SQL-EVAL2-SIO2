1. Code SQL :
   BEGIN
   UPDATE Voitures
   SET Voitures.disponible = 0
   WHERE Voitures.id = new.voiture_id;
   END

   
2. Code SQL :
   BEGIN
   SELECT Clients.age INTO @age
   FROM Clients
   WHERE Clients.id = new.client_id;

   IF @age < 21 THEN
      SIGNAL SQLSTATE "45000"
      SET MESSAGE_TEXT = "Vous devez avoir au moins 21 ans";
   END IF;
   
   END