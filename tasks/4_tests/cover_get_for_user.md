using System.Collections.Generic;
using System.Threading.Tasks;
using Xunit;

public class GoalControllerTests
{
    private readonly FakeCollections collections;

    public GoalControllerTests()
    {
        collections = new();
    }

    [Fact]
    public async Task GetGoalsForUser()
    {
        // Arrange
        var goals = collections.GetGoals();
        var users = collections.GetUsers();
        IGoalsService goalsService = new FakeGoalsService(goals, goals[0]);
        IUsersService usersService = new FakeUsersService(users, users[0]);
        GoalController controller = new(goalsService, usersService);

        // Act
        var httpContext = new Microsoft.AspNetCore.Http.DefaultHttpContext();
        controller.ControllerContext.HttpContext = httpContext;
        var result = await controller.GetGoalsForUser(goals[0].UserId!);

        // Assert
        Assert.NotNull(result);

        var userGoals = result as IEnumerable<Goal>; // Cast the result to an IEnumerable<Goal>
        Assert.NotNull(userGoals); // Ensure the cast is valid

        var index = 0;
        foreach (Goal goal in userGoals)
        {
            // Check that each goal is assignable from Goal
            Assert.IsAssignableFrom<Goal>(goal);
            // Assert that the UserId matches the expected UserId
            Assert.Equal(goals[0].UserId, goal.UserId);
            index++;
        }
    }
}
