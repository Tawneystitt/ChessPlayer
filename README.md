<h1>RTM</h1>

<h2>Description</h2>
A example of Code created as a group in CIS 411 that works as a backend and stores information in tables of Microsoft sql server, Shows use of CRUD:
<br />

<h2>Tools used:</h2>
Visio Studio, Microsoft Sql, Swagger



<p align="center">
 <br/>
  <img src="https://i.imgur.com/kAr2w4V.png"/>
<img src="https://i.imgur.com/09369LQ.png"/>




<h2/>Chess Con cs: </h2>
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using System.Diagnostics.Eventing.Reader;

namespace GroupAssign4.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ChessCon : ControllerBase
    {
        [HttpGet("{User_ID} Get User")]
        public IActionResult GetUser(string User_ID)
        {
            bool found = false;
            User user = new User();
            //check if user_Id is not null
            if (User_ID != null)
            {
                using (UserDBContext context = new UserDBContext())
                {
                    User found_user = context.User.Find(User_ID);
                    if (found_user != null)
                    {
                        //return user if found
                        return Ok(found_user);
                    }
                    else
                    { //return a 404 not found if user not found
                        return NotFound();
                    }
                }
            }
            else
            { //return 400 bad request if user_Id is null
                return BadRequest("User ID cannot be not valid.");
            }

        }

        [HttpGet("{Game_ID} Get Game")]
        public IActionResult GetGame(string Game_ID)
        {
            bool found = false;
            Chess_Game game = new Chess_Game();
            //check if game_ID is not null
        if (Game_ID != null)
        {
            using (UserDBContext context = new UserDBContext())
            {

                Chess_Game found_game = context.Chess_Game.Find(Game_ID);
                if (found_game != null)
                {
                        //return game if found
                    return Ok(found_game);
                }
                else
                { //return 404 not found if game not found
                    return NotFound();
                }
            }
        }
        else
        { //return a 400 bad request if game_ID is null
            return BadRequest("Game ID cannot be not available.");
        }
        }



        [HttpGet("{Log_ID} Get Game Log")]
        public IActionResult GetLog(string Log_ID)
        {
            bool found = false;
            Game_Log game = new Game_Log();
            //check if log_ID is not null
            if (Log_ID != null)
            {

                using (UserDBContext context = new UserDBContext())
                {
                    Game_Log found_log = context.Game_Log.Find(Log_ID);
                    if (found_log != null)
                    {
                        return Ok(found_log);
                    }
                    else
                    {
                        //return 404 not found if game log not found
                        return NotFound();
                    }
                }
            } //return bad request if log_ID is null
            else { return BadRequest("Log ID cannot be does not exist."); }
            }


        [HttpPost("Post User")]
        public IActionResult PostUser([FromBody] User user)
        {//handle post request to add a new user
            using (UserDBContext context = new UserDBContext())
            {
                if(user.Register_Date >= DateTime.Today)
                {
                    if(user.Date_of_Birth != DateTime.Now && (DateTime.Now.Year - user.Date_of_Birth.Year) > 12)
                    {
                        if(user.ELO_Score >= 0)
                        {
                            if(user.User_Name == "string")
                            {

                            }
                            context.User.Add(user);
                            context.SaveChanges();
                            return Ok(user);
                        }
                        else { return BadRequest("ELO_Score should be positive"); }
                    }
                    else { return BadRequest("User Must Be 13 Years Old"); }
                }
                else { return BadRequest("Invalid Register Date"); }
                //context.User.Add(user);
                //context.SaveChanges();
                //return Ok(user);

            }

            return BadRequest();
        }

        [HttpPost("Post Chess Game")] 
        //handle post request to add a new chess game
        public IActionResult PostGame([FromBody] Chess_Game game)
        {
            using (UserDBContext context = new UserDBContext())
            {
                context.Chess_Game.Add(game);
                context.SaveChanges();
                return Ok(game);

            }

            return BadRequest();
        }

        [HttpPost("Post Game Log")] 
        //handle post request to add a new game log
        public IActionResult PostLog([FromBody] Game_Log log)
        {
            using (UserDBContext context = new UserDBContext())
            {
                context.Game_Log.Add(log);
                context.SaveChanges();
                return Ok(log);

            }

            return BadRequest();
        }



        [HttpPut("{User_ID} Put User")] 
        //handle put request to update an exisiting user
        public IActionResult PutUser(string User_ID, [FromBody] User user)
        {
            bool found = false;
            if (User_ID != null)
            {

                using (UserDBContext context = new UserDBContext())
                {

                    User found_user = context.User.Find(User_ID);
                    if (found_user != null)
                    {
                        found_user.User_Name = user.User_Name;
                        found_user.Register_Date = user.Register_Date;
                        found_user.Date_of_Birth = user.Date_of_Birth;
                        found_user.Rank = user.Rank;
                        found_user.ELO_Score = user.ELO_Score;
                        found_user.Photo_URL = user.Photo_URL;


                        context.SaveChanges();
                        return Ok(user);
                    }
                    else
                    {
                        return NotFound();
                    }
                }
            }
            else
            {
                return BadRequest("User ID cannot be not available.");
            }
            
            }


        [HttpPut("{Game_ID} Put Game")]
        //handle put request to update an exisitng chess game
        public IActionResult PutGame(string Game_ID, [FromBody] Chess_Game game)
        {
            bool found = false;
            if (Game_ID != null)
            {

                using (UserDBContext context = new UserDBContext())
                {

                    Chess_Game found_game = context.Chess_Game.Find(Game_ID);
                    if (found_game != null)
                    {
                        found_game.Player1_ID = game.Player1_ID;
                        found_game.Player2_ID = game.Player2_ID;
                        found_game.Winning_Player_ID = game.Winning_Player_ID;
                        found_game.Start_Time = game.Start_Time;
                        found_game.End_Time = game.End_Time;
                        found_game.Game_Status = game.Game_Status;


                        context.SaveChanges();
                        return Ok(game);
                    }
                    else
                    {
                        return NotFound();
                    }
                }
            } else
            {
                return BadRequest("User ID cannot be does not exist.");
            }

        }



        [HttpDelete("{User_ID} Delete User")]
        //handle delete request ot delete an exisitng user
        public IActionResult DeleteUser(string User_ID)
        {
            if (User_ID != null)
            {
                using (UserDBContext context = new UserDBContext())
                {
                    User found_user = context.User.Find(User_ID);
                    if (found_user != null)
                    {
                        context.Remove(found_user);
                        context.SaveChanges();
                        return Ok();
                    }
                    else
                    {
                        return NotFound();
                    }

                }
            }else
            {
                return BadRequest("User ID cannot be does not exist.");
            }
        }

<h2>Chess Game. Cs</h2>
using System.ComponentModel.DataAnnotations;


namespace GroupAssign4
{
    public class Chess_Game
    {
        [Key]
        public string Game_ID {  get; set; }   //primary key for the chess game

        public string Player1_ID {  get; set; } //Id of the player 1
        public string Player2_ID { get; set; } //id of player 2
        public string Winning_Player_ID { get; set; } //id of the winning player
        public DateTime Start_Time { get; set; } //date and time the game started
        public DateTime End_Time { get; set;} //date and time the game ended
        public string Game_Status { get; set; } //status of the game

    }
}


    }

<h2>GameLog. CS:</h2>
using System.ComponentModel.DataAnnotations;

namespace GroupAssign4
{
    public class Game_Log
    {

        [Key]
        public string Log_ID {  get; set; } //primary key for the game log entry
        public string Game_ID { get; set; } //ID of the game associated with this log entry
        public string Move_Sequence { get; set; } //sequence of moves made in this game
        public string Chess_Piece { get; set; } //type of chess piece involved
        public string Start_Tile { get; set; } //starting postion of the chess piece
        public string End_Tile { get; set;} //ending position of the chess piece after the move



    }
}


<h2>User.Cs</h2>
using System.ComponentModel.DataAnnotations;

namespace GroupAssign4
{
    public class User
    {
        [Key]
        public string User_ID { get; set; } //Primary key of the user
        public string User_Name {  get; set; } //username of the user
        public DateTime Register_Date { get; set; } //date when the user registered
        public DateTime Date_of_Birth { get; set; } //dob of the user
        public string Rank { get; set; } //rank of the user
        public int ELO_Score { get; set; } //elo score of the user
        public string Photo_URL { get; set; } //user of the users photo (if available)


    }
}
<h2>UserDB Context.cs</h2>
using Microsoft.EntityFrameworkCore;

namespace GroupAssign4
{
    public class UserDBContext : DbContext //represents the database context for our application
    {
        public DbSet<User> User { get; set; } //Db set for the user entity which allows us  to interact with the user table in the db
        public DbSet<Chess_Game> Chess_Game { get; set; } //db set for the chess game entity allowing us to interact with the user table in the db
        public DbSet<Game_Log> Game_Log { get; set; } //db set for the Game log entity allowing us to interact with the user table in the db

      
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            // Configures the database connection settings to connect to a SQL Server database
            optionsBuilder.UseSqlServer(@"Data Source=cis411server.database.windows.net;Initial Catalog=CIS411Database;User ID=CIS411AppUser;Password=Password123;TrustServerCertificate=True");
        }

    }
}



