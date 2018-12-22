import pygame, math
pygame.init()

file = open("THISFILEISREAD.txt","r")
contents = file.read().split("\n")
file.close()
colors = []
backgroundColors = []
verticies3D = []
facePoints = []
color = [[255,255,255]]
message = []
windowName = "DefaultName"
for line in contents:
    list = []
    try:
        key = line[0]
        if key == "B":
            list = line[2:].split(":")
            for i in range(3):
                list[i] = int(list[i])
            for i in range(3):
                backgroundColors.append(list[i])
        if key == "V":
            list = line[2:].split(":")
            for i in range(3):
                list[i] = int(list[i])
            verticies3D.append(list)
        if key == "F":
            list = line[2:].split(":")
            for i in range(3):
                list[i] = int(list[i]) - 1
            facePoints.append(list)
            colors += color
        if key == "C":
            list = line[2:].split(":")
            for i in range(3):
                list[i] = int(list[i])
            color = [list]
        if key == "N":
            windowName = line[2:]
        if key == "M":
            message += [line[2:]]
    except:
        None

for line in message:
    print(line)

def update():
    key = pygame.key.get_pressed()
    if pygame.key.get_mods() & pygame.KMOD_LSHIFT:
        camera[0][1] -= 0.01
    if key [pygame.K_SPACE]:
       camera[0][1] += 0.01
    v, w = 0.01 * math.sin(math.radians(camera[1][1])), 0.01 * math.cos(math.radians(camera[1][1]))
    if key [pygame.K_w]:
       camera[0][0] -= v
       camera[0][2] -= w
    if key [pygame.K_s]:
        camera[0][0] += v
        camera[0][2] += w
    if key [pygame.K_a]:
        camera[0][0] += w
        camera[0][2] -= v
    if key [pygame.K_d]:
        camera[0][0] -= w
        camera[0][2] += v
    if key [pygame.K_r]:
        camera[0] = [0,0,5]
        camera[1] = [0,0,0]

def events(event):
        if event.type == pygame.MOUSEMOTION:
            v, w = event.rel
            v /= 5
            w /= 5
            camera[1][0] -= w
            camera[1][1] += v

displayCenter = [400,300]
gameDisplay = pygame.display.set_mode((2 * displayCenter[0],2 * displayCenter[1]))
pygame.display.set_caption(windowName)
font = pygame.font.SysFont("arialms",22)

pygame.event.get()
pygame.mouse.get_rel()
pygame.mouse.set_visible(0)
pygame.event.set_grab(1)

camera = [[0,0,5],[0,0]]
cameraRDS = [displayCenter[0],displayCenter[1],200]

running = True
while running:
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
            
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                running = False
        events(event)

    gameDisplay.fill((backgroundColors[0],backgroundColors[1],backgroundColors[2]))

    verticies2D = []
    for point in verticies3D:
        x = point[0] - camera[0][0] # Point X relative to the camera
        y = point[1] - camera[0][1] # Point Y relative to the camera
        z = point[2] - camera[0][2] # Point Z relative to the camera

        # sin(0) = 0; cos(0) = 1
        cosineX = math.cos(math.radians(camera[1][0])) # Cosine of Camera Angle 1
        cosineY = math.cos(math.radians(camera[1][1])) # Cosine of Camera Angle 2
        cosineZ = 1

        sineX = math.sin(math.radians(camera[1][0])) # Sine of Camera Angle 1
        sineY = math.sin(math.radians(camera[1][1])) # Sine of Camera Angle 2
        sineZ = 0

        vectorX = cosineY * (sineZ * y + cosineZ * x) - sineY * z
        vectorY = sineX * (cosineY * z + sineY * (sineZ * y + cosineZ * x)) + cosineX * (cosineZ * y - sineZ * x)
        vectorZ = cosineX * (cosineY * z + sineY * (sineZ * y + cosineZ * x)) - sineX * (cosineZ * y - sineZ * x)

        point2DX = ((cameraRDS[2] / vectorZ) * vectorX) + cameraRDS[0]
        point2DY = ((cameraRDS[2] / vectorZ) * vectorY) + cameraRDS[1]

        verticies2D += [[int(point2DX),int(point2DY)]]

    faceVerticies = []
    finalDistance = []
    for i in range(len(facePoints)):
        vertexPart = []
        for j in range(3):
            vertexPart += [verticies2D[facePoints[i][j]]]
        faceVerticies += [vertexPart]
        faceMidpoints = [(verticies3D[facePoints[i][0]][j] + verticies3D[facePoints[i][1]][j] + verticies3D[facePoints[i][2]][j]) / 3 for j in range(3)]
        partialDistance = 0
        for j in range(3):
            partialDistance += math.pow(faceMidpoints[j] - camera[0][j], 2)
        finalDistance += [[math.pow(partialDistance,0.5),i]]

    finalDistance.sort(key = lambda x: x[0],reverse = True)

    for i in range(len(facePoints)):
        pygame.draw.polygon(gameDisplay, (colors[finalDistance[i][1]][0],colors[finalDistance[i][1]][1],colors[finalDistance[i][1]][2]), (faceVerticies[finalDistance[i][1]]))

    pygame.draw.line(gameDisplay,(0,0,0),[displayCenter[0], displayCenter[1] + 5],[displayCenter[0], displayCenter[1] - 5], 1)
    pygame.draw.line(gameDisplay,(0,0,0),[displayCenter[0] + 5, displayCenter[1]],[displayCenter[0] - 5, displayCenter[1]], 1)

    update()

    x_text = font.render("Camera X: " + str(int(camera[0][0] * 100) / 100), True, (0, 0, 0))
    y_text = font.render("Camera Y: " + str(int(camera[0][1] * 100) / 100), True, (0, 0, 0))
    z_text = font.render("Camera Z: " + str(int(camera[0][2] * 100) / 100), True, (0, 0, 0))
    gameDisplay.blit(x_text,(500, 500))
    gameDisplay.blit(y_text,(500, 525))
    gameDisplay.blit(z_text,(500, 550))
    
    a_text = font.render("Camera Angle 1: " + str(int((camera[1][0] * 10) / 10) % 360), True, (0, 0, 0))
    b_text = font.render("Camera Angle 2: " + str(int((camera[1][1] * 10) / 10) % 360), True, (0, 0, 0))
    gameDisplay.blit(a_text,(100, 500))
    gameDisplay.blit(b_text,(100, 525))

    nearest_1 = font.render("Nearest Face: " + str(finalDistance[0][1]), True, (0, 0, 0))
    nearest_2 = font.render("2nd Nearest Face: " + str(finalDistance[1][1]), True, (0, 0, 0))
    nearest_3 = font.render("3rd Nearest Face: " + str(finalDistance[2][1]), True, (0, 0, 0))
    gameDisplay.blit(nearest_1,(50, 25))
    gameDisplay.blit(nearest_2,(250, 25))
    gameDisplay.blit(nearest_3,(500, 25))

    pygame.display.update()

pygame.quit()
