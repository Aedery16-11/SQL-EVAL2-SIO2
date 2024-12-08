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

3. Code SQL (triche) :
   BEGIN
   IF NEW.permis_conduire IS NULL THEN
   SIGNAL SQLSTATE '45000'
   SET MESSAGE_TEXT = 'Un numéro de permis de conduire valide est requis pour l''inscription.';
   END IF;

   IF LENGTH(NEW.permis_conduire) != 15 THEN
   SIGNAL SQLSTATE '45000'
   SET MESSAGE_TEXT = 'Le numéro de permis de conduire doit avoir exactement 15 caractères.';
   END IF;

   IF NEW.permis_conduire NOT REGEXP '^[A-Za-z0-9]{15}$' THEN
   SIGNAL SQLSTATE '45000'
   SET MESSAGE_TEXT = 'Le numéro de permis de conduire doit être alphanumérique et avoir 15 caractères.';
   END IF;

   IF EXISTS (SELECT 1 FROM Clients WHERE permis_conduire = NEW.permis_conduire) THEN
   SIGNAL SQLSTATE '45000'
   SET MESSAGE_TEXT = 'Ce numéro de permis de conduire est déjà utilisé par un autre client.';
   END IF;
   END


4. Code SQL :
   BEGIN

   SELECT Voitures.disponible INTO @dispo
   FROM Voitures
   WHERE Voitures.id = new.voiture_id;
   
   IF @dispo = 0 THEN
      SIGNAL SQLSTATE "45000"
      SET MESSAGE_TEXT = "La voiture demandée n'est pas disponible.";
   END IF;
   
   END