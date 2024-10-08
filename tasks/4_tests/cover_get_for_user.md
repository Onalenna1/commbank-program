public class GoalControllerTests
{
    private readonly FakeCollections collections;

    public GoalControllerTests()
    {
        collections = new();
    }

    [Fact]
    public async Task GetForUser_ReturnsGoalsForSpecifiedUser()
    {
        // Arrange
        var goals = collections.GetGoals();
        var users = collections.GetUsers();

        IGoalsService goalsService = new FakeGoalsService(goals, goals[0]);
        IUsersService usersService = new FakeUsersService(users, users[0]);
        GoalController controller = new(goalsService, usersService);

        // Prepare the HTTP context
        var httpContext = new Microsoft.AspNetCore.Http.DefaultHttpContext();
        controller.ControllerContext.HttpContext = httpContext;

        // Act
        var result = await controller.GetForUser(goals[0].UserId!);

        // Assert
        Assert.NotNull(result); // Assert that result is not null

        // Verify each goal's type and ownership
        foreach (var goal in result)
        {
            Assert.IsAssignableFrom<Goal>(goal); // Assert that each item is a Goal
            Assert.Equal(goals[0].UserId, goal.UserId); // Assert that the UserId matches
        }
    }
}
