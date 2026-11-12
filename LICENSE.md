
# exe 만드는 법 (Python 설치 없이 쓰기, 처음 한 번만)

1. https://github.com 에 로그인(계정 없으면 무료 가입) → 오른쪽 위 `+` → **New repository**
   이름 `scenario2video`, **Private** 선택 → Create repository
2. 만든 저장소 화면에서 **uploading an existing file** 클릭 → 이 폴더 안의 파일과 `.github` 폴더 전체를 끌어다 놓기 → **Commit changes**
   - `.github` 폴더가 안 올라가면: Add file → Create new file → 이름칸에 `.github/workflows/build.yml` 입력 후 build.yml 내용을 붙여넣기
3. 위쪽 **Actions** 탭 → 왼쪽 `build-windows-exe` → **Run workflow** (자동으로 돌기 시작하기도 함)
4. 약 3~5분 후 초록색 체크가 뜨면 그 실행을 클릭 → 맨 아래 **Artifacts** 의 `scenario2video-windows` 다운로드
5. 압축을 풀면 `scenario2video.exe`, ffmpeg, `inbox` 폴더가 들어 있습니다.

## 사용
- `scenario2video.exe` 더블클릭 → 켜져 있는 동안 `inbox` 폴더에 시나리오(.txt .docx)를 넣으면 자동 변환, 결과는 `outbox`
- 또는 시나리오 파일을 `scenario2video.exe` 위로 끌어다 놓기 (파일 1개 변환)
- Windows가 "알 수 없는 게시자" 경고를 띄우면 '추가 정보 → 실행'을 누르세요(직접 만든 프로그램이라 서명이 없어서 나오는 경고입니다).
- 음성 생성에는 인터넷이 필요합니다.

# 시나리오 → 영상 자동 변환 프로그램

시나리오 글 파일을 넣으면 **한국어 나레이션 음성 + 자막 + 슬라이드 화면**이 들어간 MP4를 자동으로 만듭니다.
무료(edge-tts 한국어 음성 사용), 크레딧 없음, 길이 제한 없음.

## 1. 한 번만 설치 (Windows/Mac 공통)
1. Python 3.9+ 설치 (https://www.python.org, Windows는 설치 시 "Add to PATH" 체크)
2. ffmpeg 설치
   - Windows: 명령 프롬프트에서 `winget install ffmpeg`
   - Mac: `brew install ffmpeg`
3. 이 폴더에서 `pip install edge-tts pillow` (docx 파일을 쓰려면 `python-docx`도)

## 2. 사용
**방법 A: 파일 하나 변환**
```
python scenario2video.py 시나리오.txt
```
→ `outbox` 폴더에 `시나리오.mp4`와 자막 `시나리오.srt`가 생깁니다.

**방법 B: 폴더에 넣으면 자동 변환 (계속 사용)**
```
python scenario2video.py --watch
```
→ `inbox` 폴더에 시나리오(.txt .md .docx)를 넣으면 자동으로 변환하고, 결과는 `outbox`에 저장됩니다. 처리된 원본은 `inbox/done`으로 이동합니다. 켜 둔 동안 계속 동작합니다(종료 Ctrl+C).

## 3. 시나리오 작성 규칙
```
1번 표지: 14회차 · 상급 · DRG 연계     ← '숫자+번'으로 시작하는 줄 = 새 장면
읽을 나레이션 문장들...                 ← 이 문장들이 음성과 자막이 됩니다
화면: 화면에 띄울 항목                  ← (선택) 여러 줄 가능, 문장이 진행되며 하나씩 나타남
[약어 안내 — 정식 명칭 및 발음]         ← (선택) 아래 줄들은 읽지 않고 화면에만 표시
DRG: Diagnosis Related Group / 진단명 기준 환자군
```
- **대화 모드**: 줄 앞에 `A:` `B:`를 붙이면 두 목소리가 번갈아 읽습니다.
- 샘플: `sample_scenario.txt`

## 4. 옵션
- `--voice ko-KR-InJoonNeural` : 남성 음성 (기본 ko-KR-SunHiNeural 여성)
- `--voice2 ...` : 대화 모드 B 화자 음성
- `--rate +10%` / `--rate -10%` : 말 속도
- `--out 폴더` : 출력 폴더 지정
- `--mock-tts` : 무음 테스트(인터넷 불필요)

## 5. 참고
- 음성 생성에는 인터넷 연결이 필요합니다.
- 화면 스타일(색, 글꼴, 레이아웃)은 `scenario2video.py` 위쪽 설정값과 `make_frame` 함수에서 바꿀 수 있습니다.

1번 표지: 14회차 · 상급 · DRG 연계

14회차 시작하겠습니다. 오늘은 상급 마지막 이론 회차, DRG 연계입니다. DRG는 Diagnosis Related Group, 진단명을 기준으로 입원 환자를 비슷한 자원을 쓰는 묶음으로 나누는 방식이에요. 오늘 다룰 건 우리나라 7개 질병군 포괄수가제입니다. 코더가 쓴 코드 한 줄이 돈의 묶음을 바꾸는 날이라, 평소보다 손이 조금 무거울 수 있습니다. 먼저 오늘의 학습목표부터 보겠습니다.

화면: 우리나라 7개 질병군 포괄수가제
화면: 백내장 · 편도 · 맹장 · 항문 · 탈장 · 제왕절개 · 자궁수술

[약어 안내 — 정식 명칭 및 발음]
DRG: Diagnosis Related Group / 진단명 기준 환자군 / 다이애그노시스 릴레이티드 그룹

2번 학습목표

학습목표입니다. 네 가지예요. 하나, 7개 질병군을 수술과 진단으로 구분합니다. 둘, 포괄수가제와 KDRG, Korean DRG, 신포괄지불제도를 헷갈리지 않습니다. 셋, 적용 제외와 분리청구가 생기는 순간을 알아봅니다. 넷, 코드가 애매하면 추정하지 않고 확인해서 질의합니다. 코더의 손에서 질병군이 정해진다는 걸 기억하면서 따라와 주세요.

화면: 1  7개 질병군을 수술과 진단으로 구분
화면: 2  포괄수가제 · KDRG · 신포괄지불제도 구분
화면: 3  적용 제외와 분리청구가 생기는 순간
화면: 4  애매하면 추정하지 않고 확인해서 질의

3번 오늘의 구성

오늘은 네 단계입니다. 임상설명에서 일곱 질병군을 하나씩 훑고, 분류코드 연계에서 색인과 본표 조회 경로를 연습하고, 문제 네 개를 푼 다음 해설과 Q&A로 마무리해요. 문제마다 POA, Present on Admission, 입원 시 상병 표시가 붙습니다. 그전에 지난 시간을 복습하고 가겠습니다.

4번 지난 시간 복습

지난 시간 복습입니다. 12회차는 뇌경색이었죠. 13회차는 패혈증이었고, 감염에 SOFA, Sequential Organ Failure Assessment 2점 이상 상승이 핵심이었습니다. 갈림길이 바뀝니다. 지난 시간은 장기부전이냐였다면, 오늘은 이 입원이 포괄수가 대상 질병군이냐입니다. 임상설명으로 가겠습니다.

#!/usr/bin/env python3
"""시나리오 글 파일 -> 나레이션 음성 + 자막 + 슬라이드 영상(MP4) 자동 변환.

사용법
  python scenario2video.py 시나리오.txt                # 파일 하나 변환
  python scenario2video.py --watch                      # inbox 폴더 감시: 파일을 넣으면 자동 변환
  python scenario2video.py 시나리오.txt --voice ko-KR-InJoonNeural --rate +10%

시나리오 형식 (README 참고)
  1번 표지: 제목            <- '번호+번' 으로 시작하는 줄이 새 장면(슬라이드)
  나레이션 문장들...         <- 이 문장들이 음성/자막이 됩니다
  화면: 화면에 띄울 항목      <- (선택) 화면 키워드. 여러 줄 가능
  [약어 안내 ...]            <- (선택) 이 줄 아래 약어 설명 줄들은 읽지 않고 화면에만 표시
  A: 대사 / B: 대사          <- (선택) 대화 모드: 두 목소리가 번갈아 읽음
"""
import argparse, asyncio, os, re, shutil, subprocess, sys, tempfile, time

W, H = 1920, 1080
BG, INK, GOLD, PANEL = (18, 32, 56), (245, 238, 220), (205, 170, 90), (30, 50, 82)
FONT_CANDIDATES = [
    "C:/Windows/Fonts/malgun.ttf", "C:/Windows/Fonts/malgunbd.ttf",
    "/System/Library/Fonts/AppleSDGothicNeo.ttc", "/Library/Fonts/AppleGothic.ttf",
    "/usr/share/fonts/truetype/nanum/NanumGothic.ttf",
    "/usr/share/fonts/truetype/noto/NotoSansCJK-Regular.ttc",
    "/usr/share/fonts/opentype/noto/NotoSansCJK-Regular.ttc",
    "/usr/share/fonts/truetype/wqy/wqy-zenhei.ttc",
]
HEAD = re.compile(r"^\s*(\d+)\s*번\s*[:：]?\s*(.*)$")
SENT = re.compile(r"(?<=[.?!。])\s+")


def die(msg):
    print("오류:", msg); sys.exit(1)


# ---------------------------------------------------------------- 파싱
def read_text(path):
    ext = os.path.splitext(path)[1].lower()
    if ext == ".docx":
        try:
            import docx
        except ImportError:
            die("docx 파일은 'pip install python-docx' 후 사용하거나 txt로 저장해 주세요.")
        return "\n".join(p.text for p in docx.Document(path).paragraphs)
    for enc in ("utf-8-sig", "utf-8", "cp949"):
        try:
            return open(path, encoding=enc).read()
        except UnicodeDecodeError:
            pass
    die("파일 인코딩을 읽을 수 없습니다. UTF-8 txt로 저장해 주세요.")


def parse(text):
    slides, cur, in_abbr = [], None, False
    for raw in text.splitlines():
        line = raw.strip()
        m = HEAD.match(line)
        if m:
            label, _, rest = m.group(2).partition(":")
            if not rest:
                label, rest = "", m.group(2)
            cur = dict(no=m.group(1), label=label.strip(), title=rest.strip() or label.strip(),
                       narr=[], points=[], abbr=[])
            slides.append(cur); in_abbr = False
            continue
        if cur is None:
            continue
        if not line:
            in_abbr = False
            continue
        if line.startswith("[약어"):
            in_abbr = True; continue
        if in_abbr:
            cur["abbr"].append(line); continue
        if line.startswith("화면:") or line.startswith("화면："):
            cur["points"].append(line.split(":", 1)[-1].split("：", 1)[-1].strip()); continue
        cur["narr"].append(line)
    for s in slides:
        s["sents"] = []
        for para in s["narr"]:
            speaker = 0
            m = re.match(r"^([AB])\s*[:：]\s*(.*)$", para)
            if m:
                speaker, para = (0 if m.group(1) == "A" else 1), m.group(2)
            for x in SENT.split(para):
                if x.strip():
                    s["sents"].append((x.strip(), speaker))
    slides = [s for s in slides if s["sents"]]
    if not slides:
        die("장면을 찾지 못했습니다. '1번 제목' 형식의 줄과 나레이션이 필요합니다. (README 참고)")
    return slides


# ---------------------------------------------------------------- 화면
def load_font(size):
    from PIL import ImageFont
    for p in FONT_CANDIDATES:
        if os.path.exists(p):
            return ImageFont.truetype(p, size)
    die("한글 글꼴을 찾지 못했습니다. FONT_CANDIDATES에 글꼴 경로를 추가해 주세요.")


def wrap(d, text, font, maxw):
    lines, cur = [], ""
    for ch in text:
        if d.textlength(cur + ch, font=font) > maxw:
            lines.append(cur); cur = ch
        else:
            cur += ch
    return lines + ([cur] if cur else [])


def make_frame(slide, idx, total, sent, shown, path):
    from PIL import Image, ImageDraw
    im = Image.new("RGB", (W, H), BG); d = ImageDraw.Draw(im)
    d.rectangle([0, 0, 14, H], fill=GOLD)
    head = f"{slide['label']}  ·  {slide['no']}/{total}" if slide["label"] else f"{slide['no']}/{total}"
    d.text((110, 60), head, font=load_font(34), fill=GOLD)
    ty = 120
    for ln in wrap(d, slide["title"], load_font(78), 1700)[:2]:
        d.text((110, ty), ln, font=load_font(78), fill=INK); ty += 100
    d.line([(110, ty + 10), (700, ty + 10)], fill=GOLD, width=4)
    y = ty + 60
    for p in slide["points"][:shown]:
        for ln in wrap(d, p, load_font(44), 1650):
            d.text((130, y), ln, font=load_font(44), fill=INK); y += 62
        y += 20
    if slide["abbr"]:
        top = max(y + 10, 560)
        d.rectangle([110, top, W - 110, min(top + 40 + 54 * len(slide["abbr"]), 800)], fill=PANEL)
        yy = top + 20
        for a in slide["abbr"][:4]:
            d.text((140, yy), a.replace(" / ", "  —  "), font=load_font(34), fill=GOLD); yy += 54
    d.rectangle([0, 830, W, H], fill=PANEL)
    yy = 868
    for ln in wrap(d, sent, load_font(46), 1700)[:3]:
        d.text((110, yy), ln, font=load_font(46), fill=INK); yy += 66
    im.save(path)


# ---------------------------------------------------------------- 음성
def run(cmd):
    subprocess.run(cmd, check=True, stdout=subprocess.DEVNULL, stderr=subprocess.DEVNULL)


def duration(path):
    out = subprocess.run(["ffprobe", "-v", "error", "-show_entries", "format=duration",
                          "-of", "csv=p=0", path], capture_output=True, text=True, check=True).stdout
    return float(out.strip())


def tts(text, voice, rate, out, mock):
    if mock:  # 테스트용: 무음. 글자 수로 길이 추정
        run(["ffmpeg", "-y", "-f", "lavfi", "-i", "anullsrc=r=24000:cl=mono",
             "-t", f"{max(1.5, len(text) / 4.6):.2f}", out]); return
    try:
        import edge_tts
    except ImportError:
        die("'pip install edge-tts' 를 먼저 실행해 주세요.")
    last = None
    for _ in range(3):
        try:
            asyncio.run(edge_tts.Communicate(text, voice, rate=rate).save(out)); return
        except Exception as e:  # 네트워크 일시 오류 재시도
            last = e; time.sleep(2)
    die(f"음성 생성 실패(인터넷 연결 확인): {last}")


def srt_time(t):
    h, r = divmod(t, 3600); m, s = divmod(r, 60)
    return f"{int(h):02d}:{int(m):02d}:{int(s):02d},{int((s % 1) * 1000):03d}"


# ---------------------------------------------------------------- 메인 변환
def convert(src, out_dir, voices, rate, mock):
    for tool in ("ffmpeg", "ffprobe"):
        if not shutil.which(tool):
            die(f"{tool} 이 설치되어 있지 않습니다. (README의 설치 안내 참고)")
    slides = parse(read_text(src))
    name = os.path.splitext(os.path.basename(src))[0]
    os.makedirs(out_dir, exist_ok=True)
    tmp = tempfile.mkdtemp(prefix="s2v_")
    segs, srt, t = [], [], 0.0
    n_all = sum(len(s["sents"]) for s in slides); k = 0
    for i, s in enumerate(slides, 1):
        n = len(s["sents"]); P = len(s["points"])
        for j, (sent, spk) in enumerate(s["sents"]):
            k += 1
            print(f"  [{k}/{n_all}] {sent[:40]}")
            aud = os.path.join(tmp, f"a{k:04d}.{'wav' if mock else 'mp3'}")
            tts(sent, voices[spk % len(voices)], rate, aud, mock)
            png = os.path.join(tmp, f"f{k:04d}.png")
            make_frame(s, i, len(slides), sent, min(P, 1 + int(j * P / n)) if P else 0, png)
            seg = os.path.join(tmp, f"s{k:04d}.mp4")
            run(["ffmpeg", "-y", "-loop", "1", "-framerate", "24", "-i", png, "-i", aud,
                 "-af", "apad=pad_dur=0.35", "-shortest", "-c:v", "libx264", "-pix_fmt", "yuv420p",
                 "-c:a", "aac", "-ar", "44100", "-ac", "2", seg])
            dur = duration(seg)
            srt.append(f"{k}\n{srt_time(t)} --> {srt_time(t + dur)}\n{sent}\n")
            t += dur; segs.append(seg)
    lst = os.path.join(tmp, "list.txt")
    with open(lst, "w", encoding="utf-8") as fh:
        for sg in segs:
            fh.write("file '" + sg.replace("\\", "/") + "'\n")
    out = os.path.join(out_dir, name + ".mp4")
    run(["ffmpeg", "-y", "-f", "concat", "-safe", "0", "-i", lst, "-c", "copy", out])
    with open(os.path.join(out_dir, name + ".srt"), "w", encoding="utf-8") as fh:
        fh.write("\n".join(srt))
    shutil.rmtree(tmp, ignore_errors=True)
    print(f"완료: {out}  ({int(t // 60)}분 {int(t % 60)}초)")
    return out


def watch(inbox, outbox, voices, rate, mock):
    done = os.path.join(inbox, "done"); os.makedirs(done, exist_ok=True); os.makedirs(outbox, exist_ok=True)
    print(f"감시 중: '{inbox}' 폴더에 시나리오(.txt .md .docx)를 넣으세요. 종료: Ctrl+C")
    while True:
        for f in sorted(os.listdir(inbox)):
            p = os.path.join(inbox, f)
            if os.path.isfile(p) and f.lower().endswith((".txt", ".md", ".docx")):
                time.sleep(1)  # 복사가 끝나길 잠깐 대기
                print("변환 시작:", f)
                try:
                    convert(p, outbox, voices, rate, mock)
                    shutil.move(p, os.path.join(done, f))
                except SystemExit:
                    shutil.move(p, os.path.join(done, "FAILED_" + f))
        time.sleep(5)


def app_dir():
    return os.path.dirname(os.path.abspath(sys.executable if getattr(sys, "frozen", False) else __file__))


if __name__ == "__main__":
    base = app_dir()
    os.environ["PATH"] = base + os.pathsep + os.environ.get("PATH", "")  # exe 옆의 ffmpeg 사용
    frozen = getattr(sys, "frozen", False)
    ap = argparse.ArgumentParser(description="시나리오 파일을 영상으로 자동 변환")
    ap.add_argument("file", nargs="?", help="시나리오 파일(.txt/.md/.docx)")
    ap.add_argument("--watch", action="store_true", help="inbox 폴더를 감시해 자동 변환")
    ap.add_argument("--inbox", default="inbox"); ap.add_argument("--outbox", default="outbox")
    ap.add_argument("--out", default="outbox", help="출력 폴더(단일 파일 모드)")
    ap.add_argument("--voice", default="ko-KR-SunHiNeural", help="기본 음성(화자 A)")
    ap.add_argument("--voice2", default="ko-KR-InJoonNeural", help="대화 모드 화자 B 음성")
    ap.add_argument("--rate", default="+0%", help="말 속도 예: +10%% / -10%%")
    ap.add_argument("--mock-tts", action="store_true", help="테스트용 무음(인터넷 불필요)")
    a = ap.parse_args()
    voices = [a.voice, a.voice2]
    if frozen:  # exe: 폴더 경로를 exe 위치 기준으로
        a.inbox, a.outbox, a.out = (os.path.join(base, "inbox"), os.path.join(base, "outbox"),
                                    os.path.join(base, "outbox"))
    if a.file:  # 파일을 exe에 끌어다 놓거나 인자로 지정
        try:
            convert(a.file, a.out, voices, a.rate, a.mock_tts)
        finally:
            if frozen:
                input("엔터를 누르면 닫힙니다...")
    elif a.watch or frozen:  # exe를 그냥 더블클릭하면 폴더 감시 모드
        os.makedirs(a.inbox, exist_ok=True); watch(a.inbox, a.outbox, voices, a.rate, a.mock_tts)
    else:
        ap.print_help()

@echo off
cd /d "%~dp0"
python scenario2video.py --watch
pause
