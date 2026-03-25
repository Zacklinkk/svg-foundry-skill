Appendix A. Color Keywords and Syntax
Colors within SVG and CSS may be specified either as exact numerical values or by using pre-defined color keywords.

Custom solid colors can be specified in any of the following formats:

- 
#*RRGGBB*, six hexidecimal digits, with each pair representing a value between 0 (00) and 255 (FF) for each color channel

- 
#*RGB*, three hexadecimal digits, equivalent to the six-digit version with every digit duplicated

- 
rgb(*r*,*g*,*b*), with either three integers between 0 and 255, or three percentage values

- 
hsl(*h*,*s*,*l*), where the first (hue) parameter is a number representing an angle in degrees on the color wheel (relative to red), and the saturation and lightness values are percentages

Custom partially-transparent colors can be specified with one of two functions:

- 
rgba(*r*,*g*,*b*,*a*), where the first three values may either be percentages or integers and the *a* value is a decimal between 0 and 1

- 
hsla(*h*,*s*,*l*,*a*), where the parameters are a number, two percentages, then a decimal alpha value between 0 and 1

The named solid colors are listed below along with their equivalents in hexadecimal, RGB, and HSL formats.  The percentage RGB values and HSL values have been rounded to whole numbers.  In addition, the keyword transparent represents rgba(0,0,0,0) or hsla(0,0%,0%,0).

Table A-1. Named Colors in SVG and CSS

**Keyword**

**Hex value**

**RGB Decimal**

**RGB Percent**

**HSL**

**R**

**G**

**B**

**R**

**G**

**B**

**H**

**S**

**L**

AliceBlue

#f0f8ff

240

248

255

94%

97%

100%

198

6%

97%

antiqueWhite

#faebd7

250

235

215

98%

92%

84%

44

14%

91%

aqua

#00ffff

0

255

255

0%

100%

100%

180

100%

50%

aquamarine

#7fffd4

127

255

212

50%

100%

83%

168

50%

75%

azure

#f0ffff

240

255

255

94%

100%

100%

180

6%

97%

beige

#f5f5dc

245

245

220

96%

96%

86%

60

10%

91%

bisque

#ffe4c4

255

228

196

100%

89%

77%

42

23%

88%

black

#000000

0

0

0

0%

0%

0%

0

0%

0%

blanchedAlmond

#ffebcd

255

235

205

100%

92%

80%

45

20%

90%

blue

#0000ff

0

0

255

0%

0%

100%

240

100%

50%

blueViolet

#8a2be2

138

43

226

54%

17%

89%

281

81%

53%

brown

#a52a2a

165

42

42

65%

16%

16%

360

75%

41%

burlywood

#deb887

222

184

135

87%

72%

53%

43

39%

70%

cadetBlue

#5f9ea0

95

158

160

37%

62%

63%

181

41%

50%

chartreuse

#7fff00

127

255

0

50%

100%

0%

80

100%

50%

chocolate

#d2691e

210

105

30

82%

41%

12%

35

86%

47%

coral

#ff7f50

255

127

80

100%

50%

31%

25

69%

66%

cornflowerBlue

#6495ed

100

149

237

39%

58%

93%

208

58%

66%

cornsilk

#fff8dc

255

248

220

100%

97%

86%

53

14%

93%

crimson

#dc143c

220

20

60

86%

8%

24%

340

91%

47%

cyan

#00ffff

0

255

255

0%

100%

100%

180

100%

50%

darkBlue

#00008b

0

0

139

0%

0%

55%

240

100%

27%

darkCyan

#008b8b

0

139

139

0%

55%

55%

180

100%

27%

darkGoldenrod

#b8860b

184

134

11

72%

53%

4%

50

94%

38%

darkGray

#a9a9a9

169

169

169

66%

66%

66%

0

0%

66%

darkGreen

#006400

0

100

0

0%

39%

0%

120

100%

20%

darkGrey

#a9a9a9

169

169

169

66%

66%

66%

0

0%

66%

darkKhaki

#bdb76b

189

183

107

74%

72%

42%

58

43%

58%

darkMagenta

#8b008b

139

0

139

55%

0%

55%

300

100%

27%

darkOliveGreen

#556b2f

85

107

47

33%

42%

18%

73

56%

30%

darkOrange

#ff8c00

255

140

0

100%

55%

0%

43

100%

50%

darkOrchid

#9932cc

153

50

204

60%

20%

80%

288

75%

50%

darkRed

#8b0000

139

0

0

55%

0%

0%

360

100%

27%

darkSalmon

#e9967a

233

150

122

91%

59%

48%

24

48%

70%

darkSeaGreen

#8fbc8f

143

188

143

56%

74%

56%

120

24%

65%

darkSlateBlue

#483d8b

72

61

139

28%

24%

55%

255

56%

39%

darkSlateGray

#2f4f4f

47

79

79

18%

31%

31%

180

41%

25%

darkSlateGrey

#2f4f4f

47

79

79

18%

31%

31%

180

41%

25%

darkTurquoise

#00ced1

0

206

209

0%

81%

82%

180

100%

41%

darkViolet

#9400d3

148

0

211

58%

0%

83%

289

100%

41%

deepPink

#ff1493

255

20

147

100%

8%

58%

318

92%

54%

deepSkyBlue

#00bfff

0

191

255

0%

75%

100%

189

100%

50%

dimGray

#696969

105

105

105

41%

41%

41%

0

0%

41%

dimGrey

#696969

105

105

105

41%

41%

41%

0

0%

41%

dodgerBlue

#1e90ff

30

144

255

12%

56%

100%

200

88%

56%

firebrick

#b22222

178

34

34

70%

13%

13%

360

81%

42%

floralWhite

#fffaf0

255

250

240

100%

98%

94%

48

6%

97%

forestGreen

#228b22

34

139

34

13%

55%

13%

120

76%

34%

fuchsia

#ff00ff

255

0

255

100%

0%

100%

300

100%

50%

gainsboro

#dcdcdc

220

220

220

86%

86%

86%

0

0%

86%

ghostWhite

#f8f8ff

248

248

255

97%

97%

100%

240

3%

99%

gold

#ffd700

255

215

0

100%

84%

0%

55

100%

50%

goldenrod

#daa520

218

165

32

85%

65%

13%

50

85%

49%

gray

#808080

128

128

128

50%

50%

50%

0

0%

50%

green

#008000

0

128

0

0%

50%

0%

120

100%

25%

greenYellow

#adff2f

173

255

47

68%

100%

18%

75

82%

59%

grey

#808080

128

128

128

50%

50%

50%

0

0%

50%

honeydew

#f0fff0

240

255

240

94%

100%

94%

120

6%

97%

hotPink

#ff69b4

255

105

180

100%

41%

71%

320

59%

71%

indianRed

#cd5c5c

205

92

92

80%

36%

36%

360

55%

58%

indigo

#4b0082

75

0

130

29%

0%

51%

284

100%

25%

ivory

#fffff0

255

255

240

100%

100%

94%

60

6%

97%

khaki

#f0e68c

240

230

140

94%

90%

55%

57

42%

75%

lavender

#e6e6fa

230

230

250

90%

90%

98%

240

8%

94%

lavenderBlush

#fff0f5

255

240

245

100%

94%

96%

330

6%

97%

lawnGreen

#7cfc00

124

252

0

49%

99%

0%

80

100%

49%

lemonChiffon

#fffacd

255

250

205

100%

98%

80%

57

20%

90%

lightBlue

#add8e6

173

216

230

68%

85%

90%

188

25%

79%

lightCoral

#f08080

240

128

128

94%

50%

50%

360

47%

72%

lightCyan

#e0ffff

224

255

255

88%

100%

100%

180

12%

94%

lightGoldenrodYellow

#fafad2

250

250

210

98%

98%

82%

60

16%

90%

lightGray

#d3d3d3

211

211

211

83%

83%

83%

0

0%

83%

lightGreen

#90ee90

144

238

144

56%

93%

56%

120

39%

75%

lightGrey

#d3d3d3

211

211

211

83%

83%

83%

0

0%

83%

lightPink

#ffb6c1

255

182

193

100%

71%

76%

344

29%

86%

lightSalmon

#ffa07a

255

160

122

100%

63%

48%

27

52%

74%

lightSeaGreen

#20b2aa

32

178

170

13%

70%

67%

178

82%

41%

lightSkyBlue

#87cefa

135

206

250

53%

81%

98%

194

46%

75%

lightSlateGray

#778899

119

136

153

47%

53%

60%

200

22%

53%

lightSlateGrey

#778899

119

136

153

47%

53%

60%

200

22%

53%

lightSteelBlue

#b0c4de

176

196

222

69%

77%

87%

204

21%

78%

lightYellow

#ffffe0

255

255

224

100%

100%

88%

60

12%

94%

lime

#00ff00

0

255

0

0%

100%

0%

120

100%

50%

limeGreen

#32cd32

50

205

50

20%

80%

20%

120

76%

50%

linen

#faf0e6

250

240

230

98%

94%

90%

40

8%

94%

magenta

#ff00ff

255

0

255

100%

0%

100%

300

100%

50%

maroon

#800000

128

0

0

50%

0%

0%

360

100%

25%

mediumAquamarine

#66cdaa

102

205

170

40%

80%

67%

168

50%

60%

mediumBlue

#0000cd

0

0

205

0%

0%

80%

240

100%

40%

mediumOrchid

#ba55d3

186

85

211

73%

33%

83%

293

60%

58%

mediumPurple

#9370db

147

112

219

58%

44%

86%

270

49%

65%

mediumSeaGreen

#3cb371

60

179

113

24%

70%

44%

157

66%

47%

mediumSlateBlue

#7b68ee

123

104

238

48%

41%

93%

255

56%

67%

mediumSpringGreen

#00fa9a

0

250

154

0%

98%

60%

166

100%

49%

mediumTurquoise

#48d1cc

72

209

204

28%

82%

80%

179

66%

55%

mediumVioletRed

#c71585

199

21

133

78%

8%

52%

314

89%

43%

midnightBlue

#191970

25

25

112

10%

10%

44%

240

78%

27%

mintCream

#f5fffa

245

255

250

96%

100%

98%

160

4%

98%

mistyRose

#ffe4e1

255

228

225

100%

89%

88%

11

12%

94%

moccasin

#ffe4b5

255

228

181

100%

89%

71%

47

29%

85%

navajoWhite

#ffdead

255

222

173

100%

87%

68%

45

32%

84%

navy

#000080

0

0

128

0%

0%

50%

240

100%

25%

oldLace

#fdf5e6

253

245

230

99%

96%

90%

47

9%

95%

olive

#808000

128

128

0

50%

50%

0%

60

100%

25%

oliveDrab

#6b8e23

107

142

35

42%

56%

14%

72

75%

35%

orange

#ffa500

255

165

0

100%

65%

0%

47

100%

50%

orangeRed

#ff4500

255

69

0

100%

27%

0%

26

100%

50%

orchid

#da70d6

218

112

214

85%

44%

84%

301

49%

65%

paleGoldenrod

#eee8aa

238

232

170

93%

91%

67%

57

29%

80%

paleGreen

#98fb98

152

251

152

60%

98%

60%

120

39%

79%

paleTurquoise

#afeeee

175

238

238

69%

93%

93%

180

26%

81%

paleVioletRed

#db7093

219

112

147

86%

44%

58%

330

49%

65%

papayaWhip

#ffefd5

255

239

213

100%

94%

84%

46

16%

92%

peachPuff

#ffdab9

255

218

185

100%

85%

73%

38

27%

86%

peru

#cd853f

205

133

63

80%

52%

25%

40

69%

53%

pink

#ffc0cb

255

192

203

100%

75%

80%

342

25%

88%

plum

#dda0dd

221

160

221

87%

63%

87%

300

28%

75%

powderBlue

#b0e0e6

176

224

230

69%

88%

90%

184

23%

80%

purple

#800080

128

0

128

50%

0%

50%

300

100%

25%

rebeccaPurple

#663399

102

51

153

40%

20%

60%

280

67%

40%

red

#ff0000

255

0

0

100%

0%

0%

360

100%

50%

rosyBrown

#bc8f8f

188

143

143

74%

56%

56%

360

24%

65%

royalBlue

#4169e1

65

105

225

25%

41%

88%

216

71%

57%

saddleBrown

#8b4513

139

69

19

55%

27%

7%

35

86%

31%

salmon

#fa8072

250

128

114

98%

50%

45%

11

54%

71%

sandyBrown

#f4a460

244

164

96

96%

64%

38%

38

61%

67%

seaGreen

#2e8b57

46

139

87

18%

55%

34%

157

67%

36%

seashell

#fff5ee

255

245

238

100%

96%

93%

35

7%

97%

sienna

#a0522d

160

82

45

63%

32%

18%

29

72%

40%

silver

#c0c0c0

192

192

192

75%

75%

75%

0

0%

75%

skyBlue

#87ceeb

135

206

235

53%

81%

92%

190

43%

73%

slateBlue

#6a5acd

106

90

205

42%

35%

80%

255

56%

58%

slateGray

#708090

112

128

144

44%

50%

56%

200

22%

50%

slateGrey

#708090

112

128

144

44%

50%

56%

200

22%

50%

snow

#fffafa

255

250

250

100%

98%

98%

360

2%

99%

springGreen

#00ff7f

0

255

127

0%

100%

50%

160

100%

50%

steelBlue

#4682b4

70

130

180

27%

51%

71%

198

61%

49%

tan

#d2b48c

210

180

140

82%

71%

55%

44

33%

69%

teal

#008080

0

128

128

0%

50%

50%

180

100%

25%

thistle

#d8bfd8

216

191

216

85%

75%

85%

300

12%

80%

tomato

#ff6347

255

99

71

100%

39%

28%

16

72%

64%

turquoise

#40e0d0

64

224

208

25%

88%

82%

177

71%

56%

violet

#ee82ee

238

130

238

93%

51%

93%

300

45%

72%

wheat

#f5deb3

245

222

179

96%

87%

70%

47

27%

83%

white

#ffffff

255

255

255

100%

100%

100%

0

0%

100%

whiteSmoke

#f5f5f5

245

245

245

96%

96%

96%

0

0%

96%

yellow

#ffff00

255

255

0

100%

100%

0%

60

100%

50%

yellowGreen

#9acd32

154

205

50

60%

80%

20%

72

76%

50%